Lab 4 - Creating Multiple HTTP Applications per tenant using AS3
--------------------------------------------------------------------------------------------------
In this lab, we will create two simple HTTP application using AS3 within the same tenant. Afterwards, we will modify the AS3 declaration to compose and create our very own third application within the same tenant. 

**Exercise 1 - Multi-App AS3 Declaration**

#. Open Postman and locate the module 5 folder under collections on the left side of postman. Double-click the ``HTTP Multiple-Application`` file.

#. Examine the body of the AS3 declaration. Take some time to familiarize yourself with how we are declaring two HTTP applications within the same tenant. 

#. After examining the declaration, send the declaration and confirm a 200 OK response. 

#. Navigate to the BIG-IP and visually confirm the changes have been made. 

    .. image:: /_static/two_apps_confirm.jpg



**Exercise 2 - Add an Additional HTTP Application within a Multi-App Single-Tenant AS3 Declaration**

#. In Postman, locate the declaration we previously sent.

#. We want to add another HTTP Application within the same tenant. 

#. **NOTE:** In Postman, and most text-editors, you can move your cursor next to an open (or closed) brace and it will locate the corresponding closed (or open) brace. This is depicted in the following images:

    .. image:: /_static/app_begin.jpg

    |

    .. image:: /_static/app_end.jpg

 
#. We want to add another application to our ``http_tenant``. We can copy the existing declaration for an application and modify parts as needed.

    .. image:: /_static/app_template.jpg

#. We want to name the application ``http_app_3`` and configure the following:



   +---------------+------------------------------------+
   | Virtual Server| Name: 'http_vs_3'                  |
   |               | Address: 10.1.20.100               |
   +---------------+------------------------------------+
   | Pool          | Name: 'http_pool_3'                |
   |               | Members: 10.1.10.34 and 10.1.10.35 |
   +---------------+------------------------------------+



#. Modify the AS3 declaration so that our ``http_app_3`` has the appropriate information. Once modified, it should look like the following:

    .. image:: /_static/app_template_pt2.jpg

#. Send the declaration. Ensure a 200 OK response. 

    .. image:: /_static/200.jpg

#. Confirm the changes on the BIG-IP. On the left column, navigate to **Local Traffic-->Virtual Servers** and change the **partition** to **'http_tenant'**. 

#. You should see the list of 3 virtual servers. 

    .. image:: /_static/3apps.jpg

#. You can navigate to **Local Traffic-> Pools** to confirm the changes made to the Pools. 

    .. image:: /_static/3pools.jpg



**Exercise 3 - Delete HTTP Applications and Tenant**

#. In order to delete our virtual server, pool and pool members, we can simply send a POST with an empty tenant body. Since AS3 is declarative, it will notice that we are sending a POST with an empty tenant body, and will by default delete the existing virtual server, pool and pool members.

    .. image:: /_static/clear_tenant.jpg

#. In Postman, find the ``Delete Application`` file. Examine the URI and body declaration. Notice we are sending a POST to the same API endpoint, but take a close look at the JSON body.

#. The body declares a AS3 tenant called ``http_tenant``, but the body describing the state of the tenant is empty. By default, AS3 will remove the virtual server, pool and pool members. Since this would cause the entire tenant to be empty, AS3 will also remove the tenant for us.

#. Click ``Send`` and ensure a 200 OK response. Navigate back to the BIG-IP, refresh the page and confirm the changes that the tenant has been deleted.

    .. image:: /_static/delete_tenant.jpg

