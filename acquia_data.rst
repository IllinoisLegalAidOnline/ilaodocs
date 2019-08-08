=========================
Acquia Data Set
=========================

This defines the various fields availabie in Acquia's data set.

Person
=======
+-----------------+-----------------------------------------------------+
| Column ID       | Description                                         |
+=================+=====================================================+
| id              | Unique id for the person                            |
+-----------------+-----------------------------------------------------+
| engagement_score| Cumulative engagement score                         |
+-----------------+-----------------------------------------------------+
| last_touch      | Date of last recorded touch; yyyy-mm-dd hh:mm:ss.mil|
+-----------------+-----------------------------------------------------+
| first_touch     | Date of first recorded touch;yyyy-mm-dd hh:mm:ss.mil|
+-----------------+-----------------------------------------------------+
| first_time_     |  t or f                                             |
| visitor         |                                                     |
+-----------------+-----------------------------------------------------+
|anonymous_visitor| t or f                                              |
+-----------------+-----------------------------------------------------+
| do_not_track    | t or f                                              |
+-----------------+-----------------------------------------------------+
| custom_field1   | Favorite content section                            |
+-----------------+-----------------------------------------------------+
| custom_field2   | Favorite content type                               |
+-----------------+-----------------------------------------------------+
| custom_field3   | Favorite keyword (from keyword taxonomy)            |
+-----------------+-----------------------------------------------------+
| custom_field4   | Company or law school, for registered users*        |
+-----------------+-----------------------------------------------------+
| custom_field5   | Notification preferences                            |
+-----------------+-----------------------------------------------------+
| custom_field6   | Organization role*                                  |
+-----------------+-----------------------------------------------------+
| custom_field7   | Zip code*                                           |
+-----------------+-----------------------------------------------------+
| custom_field8   | Gender                                              |
+-----------------+-----------------------------------------------------+
| custom_field11  | Year born                                           |
+-----------------+-----------------------------------------------------+
| custom_field12  | User's primary role (based on permissions)          |
+-----------------+-----------------------------------------------------+
| custom_field13  | Is user tester (user tester or null                 |
+-----------------+-----------------------------------------------------+
| primary_        | Primary id of the user (can be used to see profile  |
| identifier      | in profile manager                                  |
+-----------------+-----------------------------------------------------+
| persona         | Not currently used                                  |
+-----------------+-----------------------------------------------------+
| active          | Unclear what this is for                            |
+-----------------+-----------------------------------------------------+
| last_modified_  | Last modified date                                  |
+-----------------+-----------------------------------------------------+



Known issues
--------------

* Custom fields 4,6,7 is not working

Data fields
-------------

Notification preferences (custom_field5)

* email
* text 
* nothing

Primary roles

* law student
* legal aid
* navigator
* pro bono
* public
* staff


Touches
========
The touch table contains all the sessions (touches)

+--------------------+-----------------------------------------------------+
| Column ID          | Description                                         |
+====================+=====================================================+
| id                 | Id for the touch                                    |
+--------------------+-----------------------------------------------------+
| touch_duration     | Touch duration in minutes                           |
+--------------------+-----------------------------------------------------+
| touch_duration_in  | Touch duration in seconds                           |
+--------------------+-----------------------------------------------------+
| touch_date         | Date of the touch                                   |
+--------------------+-----------------------------------------------------+
| engagement_score   | Engagement score for the touch                      |
+--------------------+-----------------------------------------------------+
| referrer           | Referrer (url or direct)                            |
+--------------------+-----------------------------------------------------+
| referrer_domain    | Referrer domain (url or direct)                     |
+--------------------+-----------------------------------------------------+
| utm_referrer       | UTM referrer if known (common options include       |
|                    | Facebook, Twitter, and Mailchimp lists              |
+--------------------+-----------------------------------------------------+
| utm_medium         | UTM medium, if known.  Common options: email, Social|
+--------------------+-----------------------------------------------------+
| utm_terms          | If known                                            |
+--------------------+-----------------------------------------------------+
| utm_content        |                                                     |
+--------------------+-----------------------------------------------------+
| utm_name           | UTM name, if known                                  |
+--------------------+-----------------------------------------------------+
| url                | Landing url                                         |
+--------------------+-----------------------------------------------------+
| url_domain         | Landing base domain                                 |
+--------------------+-----------------------------------------------------+
| search_engine      | Search engine used, if applicable (Google, Bing)    |
+--------------------+-----------------------------------------------------+
| search_terms       | Search terms used to get to site if available       |
+--------------------+-----------------------------------------------------+
| number_of_page_    | Number of pages viewed in touch                     |
| views              |                                                     |
+--------------------+-----------------------------------------------------+
| person_id          | Person id associated with this touch                |
+--------------------+-----------------------------------------------------+
| country            | Country associated with session                     |
+--------------------+-----------------------------------------------------+
| region             | State                                               |
+--------------------+-----------------------------------------------------+
| city               | City                                                |
+--------------------+-----------------------------------------------------+
| postal_code        | Detected zip code                                   |
+--------------------+-----------------------------------------------------+
| latitude           | Detected latitude                                   |
+--------------------+-----------------------------------------------------+
| longitude          | Detected longitude                                  |
+--------------------+-----------------------------------------------------+
| visitor_ip         | Detected IP address                                 |
+--------------------+-----------------------------------------------------+
| platform           | DESKTOP, MOBILE, or TABLET                          |
+--------------------+-----------------------------------------------------+
| operating_system   | Operating system of the user (Windows 10, Android   |
+--------------------+-----------------------------------------------------+     


Events
=======
The events table contains all the events for a particular touch.

+-----------------+--------------------+--------------------------------+
| Column ID       | Description                                         |
+=================+=====================================================+
| event_id        | Unique id                                           |
+-----------------+-----------------------------------------------------+
| touch_id        | Id of the touch; ties to the touch table            |
+-----------------+-----------------------------------------------------+
| person_id       | Id of the person; ties to the person table          |
+-----------------+-----------------------------------------------------+
| event_name      |  Name of the event                                  |
+-----------------+-----------------------------------------------------+
| engagement_     | Engagement score for the event; 1 in all cases      |
| score           |                                                     |
+-----------------+-----------------------------------------------------+
| event_date      | Date of the event (as yyyy-mm-dd hh:mm:ss.mil       |
+-----------------+-----------------------------------------------------+
| event_category  | Event category type                                 |
| _type           |                                                     |
+-----------------+-----------------------------------------------------+
| referrer        | Referrer (url or direct)                            |
+-----------------+-----------------------------------------------------+
| page_url        | URL of the page the event took place on             |
+-----------------+-----------------------------------------------------+
| content_type    | Custom variable for the content type                |
+-----------------+-----------------------------------------------------+
| content_section | custom variable for the legal problem area based on |
|                 | our content section taxonomy                        |
|                 | Only legal content, ADRM, and blog posts use this   |
+-----------------+-----------------------------------------------------+
| custom_field_3  | Language of the content being viewed                |
+-----------------+-----------------------------------------------------+
| custom_field_4  | Outgoing Link                                       |
+-----------------+-----------------------------------------------------+
| custom_field_5  | Service type (if the user viewed a service          |
+-----------------+-----------------------------------------------------+
| custom_field_6  | File downloaded                                     |
+-----------------+-----------------------------------------------------+
| custom_field_7  | Form submitted; tracks get-legal-help submissions   |
+-----------------+-----------------------------------------------------+
| custom_field_8  | YouTube video urls                                  |
+-----------------+-----------------------------------------------------+
| custom_field_9  | Content level (basic or advanced)                   |
+-----------------+-----------------------------------------------------+
| custom_field_10 | Content audience  (advocate or advocate, pro bono)  |
+-----------------+-----------------------------------------------------+
| custom_field_11 | Content format for legal content                    | 
+-----------------+-----------------------------------------------------+         
| custom_field_12 | Legal position (plaintiff, defendant, neutral       |
+-----------------+-----------------------------------------------------+
| content_id      | Content ID (node ID                                 |
+-----------------+-----------------------------------------------------+
| customer_site_  | Filter results by ilao_prod                         |
+-----------------+-----------------------------------------------------+

         
     
Event name
-----------

* Outgoing Link
* Form submit
* Content View
* File download

Event category types
---------------------
Other (includes content views)
Campaign click through
Campaign-action

Content types
----------------

* adrm
* article
* book
* events
* internship
* job
* landing_page
* legal_content
* location
* location_services
* opportunity
* organization
* page
* portal_main_page
* site_faq
* support_ticket
* toolbox
* toolbox_tool
* toolbox_tool_step
* triage_rules
* webform

Content formats
----------------

* Easy Form
* Easy form
* File download
* Form download
* Form link
* Guide
* How-To
* IICLE
* Link
* Teach Me
* Text article
* Video



Rankings
=========


+-----------+--------------------+--------------------------------+
| Column ID | Description        | Accepted Values                |
+===========+====================+================================+
| 37336     | Content type       | legal_content,                 |
+-----------+--------------------+--------------------------------+
| 38561     | Language           | en, es, pl                     |
+-----------+--------------------+--------------------------------+
| 39646     | Legal content type | Text article, Guide, Easy form |
|           |                    |                                |
+-----------+--------------------+--------------------------------+
| 39943     | Legal position     | Neutral, defendant, plaintiff  |
+-----------+--------------------+--------------------------------+
| 37324     | Device             | MOBILE, DESKTOP, TABLET        |
+-----------+--------------------+--------------------------------+
| 39645     | Audience           | Advocate, Pro bono             |
+-----------+--------------------+--------------------------------+
| 37337     | Content section    | See taxonomy                   |
+-----------+--------------------+--------------------------------+
| 37341     | Content keyword    | See taxonomy                   |
+-----------+--------------------+--------------------------------+
| 37301     | Engagement score   |                                |
+-----------+--------------------+--------------------------------+
| 39170     | Event label        |                                |
+-----------+--------------------+--------------------------------+
| 37346     | Event              |                                |
+-----------+--------------------+--------------------------------+
