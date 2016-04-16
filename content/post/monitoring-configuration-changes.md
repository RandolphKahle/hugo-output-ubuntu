+++
title = "Monitoring configuration changes in NetKernel transports"
description = "Understand how to create a dependency check in a NetKernel transport"
date = "2010-10-20T12:07:00-07:00"
tags = ["netkernel"]
categories = ["programming"]
menu = ""
bannerSmall  = "images/netkernel/netkernel-logo-256x256.png"
bannerMedium = "images/netkernel/netkernel-logo-256x256.png"
banner       = "images/netkernel/netkernel-logo-256x256.png"
originalurl  = "https://resourcefulcomputing.wordpress.com/2010/10/20/monitoring-configuration-changes/"

[rdfarticleimage]
  url = "http://www.resourcefulcomputing.com/images/netkernel/netkernel-logo-256x256.png"
  height = "256"
  width = "256"

[dates]
	published = "2010-10-20T12:07:00-07:00"
	modified =  "2016-02-07T12:00:00-07:00"
+++ 


In NetKernel transport endpoints can monitor their configuration information enabling a user to 
dynamically change their settings while an application is running.

I will show an implementation pattern you can use within a transport that will provide this dynamic behavior. 
The key is understanding that a transport can request its configuration information as a resource request, 
retain a reference to the response and later check to see if that response has expired. 
The response will report that it has expired when the configuration information represented by the resource has been changed.

Since I am writing a transport for the new Twitter Stream API, I will use this as the example to illustrate the pattern. 
The Twitter Stream API is relatively new and allows you to set up a filtered stream of status messages 
(formatted in JSON or XML) that are sent directly from the Twitter servers. 
An example configuration change is the addition or removal of a Twitter ID 
from those being filtered or monitored. 
By implementing this pattern my transport will detect when the file containing the configuration 
changes and will cause the transport to reconnect to Twitter with the new filter information.

For the Twitter stream transport, the configuration is available from the resource identified as 
`res:/etc/system/TwitterStreamConfig.xml`. 
he transport can request the configuration information represented as an HDS node with the following code:


	String configURI = "res:/etc/system/TwitterStreamConfig.xml";
	IHDSNode configuration = context.source(configURI, IHDSNode.class);

However, we want to maintain a reference to the response returned by the request and therefore use this code:

	IHDSNode configuration;
	INKFResponseReadOnly configurationResponse;
	String configURI = "res:/etc/system/TwitterStreamConfig.xml";
	configurationResponse = context.sourceForResponse(configURI, IHDSNode.class);
	configuration = (IHDSNode)configurationResponse.getRepresentation();


Later, we can check to see if the response associated with the endpoint configuration is still valid with a call to `isExpired()`:

 	if (configurationResponse.isExpired())
		{
		// Re-request the configuration information
		}

The easiest way to set up periodic monitoring of the configuration response is to create a monitor thread, 
which can be done in the `postCommission(...)` method. 
The corresponding `preDecommission(...)` method would contain the code to interrupt and hence terminate the monitor thread.


In the following code I show the portion portion of the `TwitterStreamTransport` class which provides the dynamic reconfigurability of the endpoint. (The part of the transport that deals with Twitter has not been included as that would obscure the illustration of this pattern.)



	package net.databliss.netkernel.tpt.twitter;

	import org.netkernel.layer0.nkf.INKFRequestContext;
	import org.netkernel.layer0.nkf.INKFResponseReadOnly;
	import org.netkernel.layer0.representation.IHDSNode;
	import org.netkernel.module.standard.endpoint.StandardTransportImpl;

	public class TwitterStreamTransport extends StandardTransportImpl
	  {
	  private static final int POLL_INTERVAL = 10000; // ms

	  // Thread monitoring changes in configuration resources
	  private Thread monitorThread;

	  // The configurationResponse is the link to the cached configuration
	  // When the response expires then we must re-request the configuration
	  // using its identifier to get the updated information.
	  private INKFResponseReadOnly configurationResponse;

	  private volatile boolean pollConfiguration = true;

	  //==================== Override Superclass Methods ==================

	  @Override
	  protected void postCommission(INKFRequestContext inkfRequestContext) throws Exception
	    {
	    // Start the configuration monitoring thread
	    monitorThread = new ConfigurationPoll();
	    monitorThread.start();
	    }

	  @Override
	  protected void preDecommission(INKFRequestContext inkfRequestContext) throws Exception
	    {
	    // Stop the configuration monitor thread by interrupting it
	    pollConfiguration = false;
	    monitorThread.interrupt();
	    }

	  //==================== Private Implementation Methods ==================

	  private void checkConfig() throws Exception
	    {
	    if (configurationResponse == null || configurationResponse.isExpired())
	      {
	      // If the configuration has never been requested or if it has expired
	      // then we need to request it using the identifier is supplied by the
	      // config parameter.
	      INKFRequestContext context = this.getTransportContext();
	      configurationResponse = context.sourceForResponse("param:config", IHDSNode.class);
	      configure((IHDSNode) configurationResponse.getRepresentation());
	      }
	    }

	  private void configure(IHDSNode configuration)
	    {
	    // Code to configure the transport is included here.
	    }

	  private class ConfigurationPoll extends Thread
	    {
	    public void run()
	      {
	      while (pollConfiguration)
	        {
	        try { checkConfig(); } catch (Exception e) { e.printStackTrace(); }
	        try { sleep(POLL_INTERVAL);	} catch (InterruptedException e) { } 
	        }
	      }
	    }
	  }

  If the response indicates that the configuration has expired, it does not mean that the configuration has new information. 
  For example, a user could edit the file and add some space characters, which would invalidate the response but would not change the configuration. 
  The private method `configure(...)` should include a test to see if the configuration information is new before making changes to the state of the transport.

