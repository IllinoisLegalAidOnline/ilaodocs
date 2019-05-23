==========================
Acquia Lift Integration
==========================

Core Configuration
===================
The core configuration is stored in the settings.php file in sites/default.  This is to ensure that unique site IDs are used for dev (including all various local development environments), staging, and production.

Our production instance has a name of ILAO Production and a site ID of ilao_prod.  Our staging instance has a name of ILAO Stage and a site ID of ilao_stage.

.. toctree::
   :maxdepth: 1
 
   acquia_data_overview
   acquia_default_data
   acquia_ilao_person_data
   acquia_ilao_touch_data
   acquia_ilao_event_data
   acquia_ilao_segments
   acquia_data
   acquia_download

Tracking
=========

Looking up your user cookie ID
--------------------------------
To find your cookie ID:
 * Open Chome
 * Go to our website
 * In Chrome, go to View/Developer tools and Application
 * Under cookies, look under www.illinoislegalaid.org
 * It is the tc_ptid cookie

.. image:: assets/cookie_image.png


