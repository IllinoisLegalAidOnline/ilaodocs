======================================
OTIS Database Tables & Relationships
======================================

The documentation below references specific queries and tables generated for reporting purposes and the relationship between them.

Determining if intake is available
==========================================
Intake is available when:
 
 * the user's zip code matches the zip code associated with an intake setting
 * the user's legal problem matches a legal issue associated with an intake setting
 * the user matches the service on population, or the service is open to all
 * intake settings is set to enabled
 * the current count of intakes is less than the number of intakes allowed or the intake limit is set to unlimited
 * there are triage rules that

   * match the service associated with an intake setting
   * match the intake settings on legal issue
   * are published
 * there is an income match on the service:
   
   * if the user indicates "no" on the starting income question, this is ignored
   * if the user indicates "yes" on the starting income quesiton, the system checks the intake settings to see if income is waived for any populations; the user must then match on the population

.. note::

   Example:
   A program may have 1 service "Housing unit" that covers all housing issues in Kane, Kendall, and DeKalb counties.  They may have 2 sets of intake settings: one for Kane county and one for Kendall & Dekalb county. Kane county is closed however. They then set up triage rules for foreclosure and associate them with the Housing Unit.  A user in Kane county would get referrals to the service and a user in DeKalb would get online intake because while there are triage rules matching the service, the intake settings controls location and those are only open for Kendall & DeKalb counties.
   
 
 

OAS Triage User
=================
OAS_triage_user is the entity that stores all the information about someone who uses
Get Legal Help, from ILAO triage through program triage and referrals/intake.

Fields
--------

+-------------------------+-------------------------------------+
|  Field                  |  Notes                              |
+=========================+=====================================+
| triage_id               | Unique ID for the session           |
+-------------------------+-------------------------------------+
| uid                     | If logged in, the user's ID         |
+-------------------------+-------------------------------------+
| age                     | Intake only                         |
+-------------------------+-------------------------------------+
| triage_changed          | Time of last triage activity        |
+-------------------------+-------------------------------------+
| citizenship             | Intake only if asked                |
+-------------------------+-------------------------------------+
| county                  | Intake, computed from zip code      |
+-------------------------+-------------------------------------+
| triage_started          | Time Get Legal Help form submitted  |
+-------------------------+-------------------------------------+
| ethnicity               | Intake only if asked                |
+-------------------------+-------------------------------------+
| gender                  | Intake only if asked                |
+-------------------------+-------------------------------------+
| household_adults        | Intake only                         |
+-------------------------+-------------------------------------+
| household_children      | Intake only                         |
+-------------------------+-------------------------------------+
| household_size          | May be empty if user skipped        |
|                         | on Get Legal Help form              |
+-------------------------+-------------------------------------+
| immigrant_status        | Intake only if asked                |
+-------------------------+-------------------------------------+
| intake_started          | Time of first intake page saved     |
+-------------------------+-------------------------------------+
| intake_changed          | Time of last intake activity        |
+-------------------------+-------------------------------------+
| last_screen_viewed      | Last page viewed by user            |
|                         | If node/number user declined modal  |
+-------------------------+-------------------------------------+
| marital_status          | Intake only if asked                |
+-------------------------+-------------------------------------+
| primary_language        | Intake only if asked                |
+-------------------------+-------------------------------------+
| overincome              | null = user skipped on GLH form     |
|                         | 0 = No                              |
|                         | 1 = Yes (Triage only)               |
|                         | 2= Over income (Intake only)        |
|                         | 3 = Over asset (Intake only)        |
+-------------------------+-------------------------------------+
| race                    | Intake only if asked                |
+-------------------------+-------------------------------------+
| state                   | Intake only; computed from zip      |
+-------------------------+-------------------------------------+
| total_income            | Intake only if asked; computed      |
+-------------------------+-------------------------------------+
| total_assets            | Intake only if asked; computed      |
+-------------------------+-------------------------------------+
| total_expenses          | Intake only if asked; computed      |
+-------------------------+-------------------------------------+
| triage_status           | Last triage status reached          |
+-------------------------+-------------------------------------+
| zip_code                | User's zip code                     |
+-------------------------+-------------------------------------+
| service_id              | Node ID of the service              |
+-------------------------+-------------------------------------+
| intake_settings_id      | ID the intake settings used         |
+-------------------------+-------------------------------------+

Triage user legal issue
-------------------------

The legal issue is the lowest level term in the legal issue taxonomy.

+--------------------------+-------------------------------------+
|  Field                   |   Notes                             |
+==========================+=====================================+
|  triage_id               |  Triage ID of the session           |
+--------------------------+-------------------------------------+
|  legal_problem           | Term ID from legal issues           |
+--------------------------+-------------------------------------+
|  legal_problem_name      | Name of the legal issue             |
+--------------------------+-------------------------------------+

Triage user populations
-------------------------

If the user identified a special population on the main Get Legal Help screen,
it shows up in this query.

+--------------------------+-------------------------------------+
|  Field                   |   Notes                             |
+==========================+=====================================+
|  triage_id               |  Triage ID of the session           |
+--------------------------+-------------------------------------+
|  term_id                 | Term ID from populations            |
+--------------------------+-------------------------------------+
|  population_name         | Label used to describe population   |
+--------------------------+-------------------------------------+


OAS Intake Settings
====================

Intake settings is the key data structure (entity) for a program's intake parameters for a service
or subset of legal issues within a service. 

These settings power the intake section once the user completes ILAO's triage
and program triage without getting diverted.

All Yes/No values are stored as 0 (No) or 1 (Yes).


+--------------------------+-----------------------------------------+
|  Field                   |   Notes                                 |
+==========================+=========================================+
|  service                 |  Name of the service tied to settings   |
+--------------------------+-----------------------------------------+
|  service_id              | Node ID of the service                  |
+--------------------------+-----------------------------------------+
|  intake_settings_id      | Intake settings unique id               |
+--------------------------+-----------------------------------------+
|  name                    |  Name of the intake settings            |
+--------------------------+-----------------------------------------+
|  allow_prisoners         | Yes/No if people in jail can apply      |
+--------------------------+-----------------------------------------+
|  apply_asset_limit       | Yes/No.  Currently unused.              |
+--------------------------+-----------------------------------------+
|  apply_income_limit      | Yes/No                                  |
+--------------------------+-----------------------------------------+
|  callback_number         | Phone number for callbacks              |
+--------------------------+-----------------------------------------+
|  callback_type           | We call client or client calls          |
+--------------------------+-----------------------------------------+
|  last_updated            | Time of last update to these settings   |
+--------------------------+-----------------------------------------+
|  collect_citizenship     | Yes/No to ask for citizenship           |
+--------------------------+-----------------------------------------+
|  collect_country         | Yes/No to ask for country of origin     |
+--------------------------+-----------------------------------------+
|  collect_language        | Yes/No to ask for primary language      |
+--------------------------+-----------------------------------------+
|  collect_gender          | Yes/No to ask for gender                |
+--------------------------+-----------------------------------------+
|  collect_race            |  Yes/No to ask for user's race          |
+--------------------------+-----------------------------------------+
|  collect_ethnicity       | Yes/No to ask for user's ethnicity      |
+--------------------------+-----------------------------------------+
|  collect_income          | Yes/No to ask for income information    |
+--------------------------+-----------------------------------------+
|  collect_assets          | Yes/No to ask user for assets           |
+--------------------------+-----------------------------------------+
|  collect_expenses        | Yes/No to ask for expense info          |
+--------------------------+-----------------------------------------+
|  created                 | Time of intake settings created         |
+--------------------------+-----------------------------------------+
|  enabled                 | Are these settings active?              |
+--------------------------+-----------------------------------------+
|  intake_limit            | Max number of intakes; 0 = unlimited    |
+--------------------------+-----------------------------------------+
|  maximum_allowed_income  | Maximum percent of allowed income       |
+--------------------------+-----------------------------------------+
|  maximum_allowed_assets  | Dollar amount of allowed assets         |
|                          | 0 is treated as unlimited               |
+--------------------------+-----------------------------------------+
|  maximum_callbacks       | Maximum number of callbacks that can be |
|  _per_slot               | scheduled for a particular hour slot    |
+--------------------------+-----------------------------------------+
|  minimum_minor_age       | Lowest allowed age to apply online      |
|                          | Default is 18; minimum allowed is 13    |
+--------------------------+-----------------------------------------+
|  minimum_senior_age      | Lowest age to be considered a senior    |
+--------------------------+-----------------------------------------+
| reset_limit_frequency    | How often intake limits are reset       |
+--------------------------+-----------------------------------------+
|  personal_exemption      | Dollar amount of personal property that |
|  _amount                 | is deducted from assets before          |
|                          | calculating asset eligibility.          |
+--------------------------+-----------------------------------------+
| income_standard          |  Income standard to apply               |
+--------------------------+-----------------------------------------+
| geographic_reach         | Indicates whether intake settings are   |
|                          | limited at the city or county level or  |
|                          | are statewide.                          |
+--------------------------+-----------------------------------------+

Intake settings & Geographic Eligibility
-----------------------------------------
Programs can set intake settings to be available to the same geographic
region as the associated service or they can limit intake to a different
region.

The `three queries <otis_queries.html#queries-for-geographic-reach>`_ under geographic reach can be used to generate the
list of zip codes for services by geographic_reach.

+--------------------------+-----------------------------------------+
|  Field                   |   Notes                                 |
+==========================+=========================================+
|  intake_settings_id      | Intake settings id                      |
+--------------------------+-----------------------------------------+
|  term_id                 | Term ID of the zip code                 |
+--------------------------+-----------------------------------------+
|  zip_code                | Zip code                                |
+--------------------------+-----------------------------------------+



Intake settings & legal issue
------------------------------
The intake settings legal issue controls whether an intake setting applies to a given session.



Financial Categories
-----------------------
If the program wants to collect income, assets, and/or expenses, we then ask them to pick
from a list of financial categories in each class.  This information is stores in its
own tables. 

Assets
^^^^^^

+--------------------------+-----------------------------------------+
|  Field                   |   Notes                                 |
+==========================+=========================================+
|  intake_settings_id      | Intake settings id                      |
+--------------------------+-----------------------------------------+
|  intake_settings_name    | Name of the intake settings             |
+--------------------------+-----------------------------------------+
|  financial_id            | ID of the asset                         |
+--------------------------+-----------------------------------------+
|  asset                   | Name of the asset to be collected       |
+--------------------------+-----------------------------------------+
|  subcategory             | Grouping the asset belongs to when      |
|                          | displayed to the user.                  |
+--------------------------+-----------------------------------------+

Income
^^^^^^

+--------------------------+-----------------------------------------+
|  Field                   |   Notes                                 |
+==========================+=========================================+
|  intake_settings_id      | Intake settings id                      |
+--------------------------+-----------------------------------------+
|  intake_settings_name    | Name of the intake settings             |
+--------------------------+-----------------------------------------+
|  financial_id            | ID of the income item                   |
+--------------------------+-----------------------------------------+
|  income                  | Name of the income item to be collected |
+--------------------------+-----------------------------------------+
|  subcategory             | Grouping the income item belongs to     |
|                          | when displayed to the user.             |
+--------------------------+-----------------------------------------+

Expenses
^^^^^^^^

+--------------------------+-----------------------------------------+
|  Field                   |   Notes                                 |
+==========================+=========================================+
|  intake_settings_id      | Intake settings id                      |
+--------------------------+-----------------------------------------+
|  intake_settings_name    | Name of the intake settings             |
+--------------------------+-----------------------------------------+
|  financial_id            | ID of the expense                       |
+--------------------------+-----------------------------------------+
|  expense                 | Name of the expense to be collected     |
+--------------------------+-----------------------------------------+
|  subcategory             | Grouping the expense belongs to         |
|                          | when displayed to the user.             |
+--------------------------+-----------------------------------------+

Population Parameters
----------------------
There are 2 ways population impacts the intake settings:

 * If the associated service is limited to a population, so is the intake setting.
 * Regardless of the service, a program can waive income rules to specific populations.
   This information is in the intake settings.

Triage Rules
=============
Triage rules are a type of website content that allows programs to write their own triage
rules.  Triage rules are related to:

 * Services through a field that maps the triage rules to one or more services.  A program may choose to use one set of triage rules for 6 services; they would still need to have 6 separate intake settings (one per service).
 * Intake settings through the legal issues selected.  A program may have multiple sets of triage rules that cover different legal issues but still use the same set of intake settings.
 
List of triage rules
----------------------
 
+--------------------------+-----------------------------------------+
|  Field                   |   Notes                                 |
+==========================+=========================================+
|  nid                     | Node ID of the triage rules             |
+--------------------------+-----------------------------------------+
|  title                   | Name of the triage rules                |
+--------------------------+-----------------------------------------+
|  status                  | 0 = unpublished                         |
|                          | 1 = published                           |
+--------------------------+-----------------------------------------+
|  created                 | Date/time initially created             |
+--------------------------+-----------------------------------------+
|  last_update             | Date/time last changed                  |
+--------------------------+-----------------------------------------+

With associated services
------------------------

+--------------------------+-----------------------------------------+
|  Field                   |   Notes                                 |
+==========================+=========================================+
|  nid                     | Node ID of the triage rules             |
+--------------------------+-----------------------------------------+
|  title                   | Name of the triage rules                |
+--------------------------+-----------------------------------------+
|  status                  | 0 = unpublished                         |
|                          | 1 = published                           |
+--------------------------+-----------------------------------------+
|  created                 | Date/time initially created             |
+--------------------------+-----------------------------------------+
|  last_update             | Date/time last changed                  |
+--------------------------+-----------------------------------------+
|  service _id             | Node ID for the service                 |
+--------------------------+-----------------------------------------+
|  service_title           | Title of the associated service         |
+--------------------------+-----------------------------------------+

With associated legal issues
----------------------------

+--------------------------+-----------------------------------------+
|  Field                   |   Notes                                 |
+==========================+=========================================+
|  nid                     | Node ID of the triage rules             |
+--------------------------+-----------------------------------------+
|  title                   | Name of the triage rules                |
+--------------------------+-----------------------------------------+
|  status                  | 0 = unpublished                         |
|                          | 1 = published                           |
+--------------------------+-----------------------------------------+
|  created                 | Date/time initially created             |
+--------------------------+-----------------------------------------+
|  last_update             | Date/time last changed                  |
+--------------------------+-----------------------------------------+
|  term_id                 | Term ID of the legal issue              |
+--------------------------+-----------------------------------------+
|  legal_issue             | Name of the legal issue                 |
+--------------------------+-----------------------------------------+

Webform components
----------------------

Webform components are the individual questions, etc asked or shown in a set of triage rules.


+--------------------------+-----------------------------------------+
|  Field                   |   Notes                                 |
+==========================+=========================================+
|  triage_rules_id         | Node ID of the triage rules             |
+--------------------------+-----------------------------------------+
|  component_id            | Unique id for the component on a        |
|                          | specific webform.  Generally sequential |
+--------------------------+-----------------------------------------+
|  form_key                | Key as stored by webform                |
+--------------------------+-----------------------------------------+
|  name                    | Administrative label; also displayed to |
|                          | to users when a question                |
+--------------------------+-----------------------------------------+
|  type                    | The type of form element.  See below    |
+--------------------------+-----------------------------------------+
|  value                   | For markup, the text to display         |
|                          | For hidden elements, the value to save  |
+--------------------------+-----------------------------------------+
|  status                  | Whether the triage rules is published   |
+--------------------------+-----------------------------------------+

Triage form element types
^^^^^^^^^^^^^^^^^^^^^^^^^^

 * hidden:  these do not display to users. 
   * End point defines the exit point (intake, divert, bypass)
   * Notes is used to store text that gets appended to case notes in an etransfer
   
* Select: radio select question; user must pick an option
* textfield: text input; user enters some text.
* yesno: radio question with only a Yes/No option
* markup: displays a message to the user without a question
* date: date field

Webform conditionals
---------------------
Webform conditionals allow us to add branching logic to triage rules' components.

For example, if I create a yesno component "Are you married?" and another component
"end point" of type hidden, I might have a conditional like:

 * When [are you married] equals 1 (value), set end point (action) to Divert (argument).
 * When [are you married] equals 0 (value), set end point (action) to Intake  (argument).
 
+--------------------------+-----------------------------------------+
|  Field                   |   Notes                                 |
+==========================+=========================================+
|  nid                     | Node ID of the triage rules             |
+--------------------------+-----------------------------------------+
|  rgid                    | Unique id for the conditional rule group|
+--------------------------+-----------------------------------------+
|  rid                     | Unique id for the rule                  |
+--------------------------+-----------------------------------------+
|  source_type             | This is always "component"              |
+--------------------------+-----------------------------------------+
|  source                  | The component number                    |
+--------------------------+-----------------------------------------+
|  operator                | Operator for the rule.  Example         |
|                          | [component] equals [value], equals is   |
|                          | the operator                            |
+--------------------------+-----------------------------------------+
|  value                   | Value to evaluate condition for         |
+--------------------------+-----------------------------------------+
|  action                  | Action to take.                         |
+--------------------------+-----------------------------------------+
|  argument                | The value for the action                |
+--------------------------+-----------------------------------------+

Location Services
==================    

List of services with their location and organization
------------------------------------------------------

Each service is tied to exactly 1 location and that location is tied to exactly 1 
organization.

+--------------------------+-----------------------------------------+
|  Field                   |   Notes                                 |
+==========================+=========================================+
|  service_id              | Node ID of the service                  |
+--------------------------+-----------------------------------------+
|  service_title           | Name of the service                     |
+--------------------------+-----------------------------------------+
|  status                  | Status of the service                   |
|                          | 0 = unpublished                         |
|                          | 1 = published                           |
+--------------------------+-----------------------------------------+
|  location_id             | Node id of the location of the service  |
+--------------------------+-----------------------------------------+
|  location                | Name of the location                    |
+--------------------------+-----------------------------------------+
|  organization_id         | Node id of the organization             |
+--------------------------+-----------------------------------------+
|  organization_nam        | Name of the organization                |
+--------------------------+-----------------------------------------+

List of services limited to specific populations
-------------------------------------------------

+--------------------------+-----------------------------------------+
|  Field                   |   Notes                                 |
+==========================+=========================================+
|  service_id              | Node ID of the service                  |
+--------------------------+-----------------------------------------+
|  service_title           | Name of the service                     |
+--------------------------+-----------------------------------------+
|  status                  | Status of the service                   |
|                          | 0 = unpublished                         |
|                          | 1 = published                           |
+--------------------------+-----------------------------------------+
|  term_id                 | Term id of the population               |
+--------------------------+-----------------------------------------+
| population               | Name of the population                  |
+--------------------------+-----------------------------------------+




List of services with their legal issues
-----------------------------------------

+--------------------------+-----------------------------------------+
|  Field                   |   Notes                                 |
+==========================+=========================================+
|  service_id              | Node ID of the service                  |
+--------------------------+-----------------------------------------+
|  service_title           | Name of the service                     |
+--------------------------+-----------------------------------------+
|  status                  | Status of the service                   |
|                          | 0 = unpublished                         |
|                          | 1 = published                           |
+--------------------------+-----------------------------------------+
|  term_id                 | Term id of the legal issue              |
+--------------------------+-----------------------------------------+
|  legal_issue             | Name of the legal issue                 |
+--------------------------+-----------------------------------------+


Services by geographic coverage
---------------------------------
Services by geographic coverage require running multiple queries since
that data can be stored at the state, county, city or zip level.

The format for all of this data is the same:

+--------------------------+-----------------------------------------+
|  Field                   |   Notes                                 |
+==========================+=========================================+
|  nid                     | Node ID of the service                  |
+--------------------------+-----------------------------------------+
|  service_title           | Name of the service                     |
+--------------------------+-----------------------------------------+
|  status                  | Status of the service                   |
|                          | 0 = unpublished                         |
|                          | 1 = published                           |
+--------------------------+-----------------------------------------+
|  term_id                 | Term id of the zip code                 |
+--------------------------+-----------------------------------------+
|  zip_code                | Zip code                                |
+--------------------------+-----------------------------------------+




Services vs intake settings geographic coverage
------------------------------------------------


+--------------------------+-----------------------------------------+
|  Field                   |   Notes                                 |
+==========================+=========================================+
|  intake_settings         | Name of the intake settings             |
+--------------------------+-----------------------------------------+
| intake_settings_id       | ID of the intake settings               |
+--------------------------+-----------------------------------------+
|  status                  | Status of the intake_settings           |
|                          | 0 = unpublished                         |
|                          | 1 = published                           |
+--------------------------+-----------------------------------------+
|  has_same_geographic     | Does the intake settings use the same   |
|  _area                   | geographic region as service?           |
+--------------------------+-----------------------------------------+
|  service_id              | Node ID of the service                  |
+--------------------------+-----------------------------------------+
| service_name             | Name of the service                     |
+--------------------------+-----------------------------------------+
| service_status           | Status of the service                   |
|                          | 0 = unpublished                         |
|                          | 1 = published                           |
+--------------------------+-----------------------------------------+

Services by fees
------------------------

+--------------------------+-----------------------------------------+
|  Field                   |   Notes                                 |
+==========================+=========================================+
|  service_id              | Node ID of the service                  |
+--------------------------+-----------------------------------------+
| service_name             | Name of the service                     |
+--------------------------+-----------------------------------------+
| statys                   | Status of the service                   |
|                          | 0 = unpublished                         |
|                          | 1 = published                           |
+--------------------------+-----------------------------------------+
| income_eligibility       | Free to everyone                        |
|                          | Free to eligible persons                |
|                          | Sliding scale                           |
|                          | Flat free                               |
+--------------------------+-----------------------------------------+

Free to everyone and free to eligible persons take priority over sliding scale and flat fees.




