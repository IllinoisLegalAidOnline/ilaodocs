================================
OTIS-related queries
================================

Legal issues with any triage rules
===================================
Returns all triage rules with their associated taxonomy terms.

.. code-block:: sql

   Select distinct taxonomy_entity_index.entity_id,
   taxonomy_entity_index.tid,
   taxonomy_term_data.name,
   field_data_title_field.title_field_value
   from taxonomy_entity_index
   inner join taxonomy_term_data
   on taxonomy_entity_index.tid = taxonomy_term_data.tid
   inner join node on nid = taxonomy_entity_index.entity_id
   inner join field_data_title_field on field_data_title_field.entity_id = nid
   where taxonomy_entity_index.bundle = 'triage_rules'
   and field_name = 'field_legal_issues'

Legal issues with published triage rules
=========================================
.. code-block:: sql

   SELECT DISTINCT taxonomy_entity_index.entity_id,
   taxonomy_entity_index.tid,
   taxonomy_term_data.name,
   field_data_title_field.title_field_value
   from taxonomy_entity_index
   inner join taxonomy_term_data
   on taxonomy_entity_index.tid = taxonomy_term_data.tid
   inner join node on nid = taxonomy_entity_index.entity_id
   inner join field_data_title_field
   on field_data_title_field.entity_id = nid
   where taxonomy_entity_index.bundle = 'triage_rules'
   AND field_name = 'field_legal_issues' 
   AND node.status = 1

Legal issues with no triage rules, regardless of level
=======================================================
The tid not in clause excludes terms under About Lawyers, Courts & Hearings, and Practice Resources.

.. code-block:: sql

   SELECT tid, name from taxonomy_term_data where tid not in
   (SELECT taxonomy_entity_index.tid from taxonomy_entity_index
   inner join taxonomy_term_data
   on taxonomy_entity_index.tid = taxonomy_term_data.tid
   inner join node on nid = taxonomy_entity_index.entity_id
   inner join field_data_title_field on field_data_title_field.entity_id = nid
   WHERE taxonomy_entity_index.bundle = 'triage_rules'
   AND field_name = 'field_legal_issues' and node.status = 1)
   AND vid = 61 and tid not in
   (517836, 517841, 517846, 524176, 524156, 524161, 524221,
   524171, 524211, 524181, 524236, 524306, 517851, 517856,
   517866, 517896, 517906, 517901, 522986, 517911, 517861,
   517921, 517936, 517926, 517916, 517931, 517941, 517946,
   522646, 517951, 517956, 517966, 517976, 517961, 517971,
    517871, 517876, 517886, 517891, 517881) 


Legal issues with no triage rules, lowest level terms only
==========================================================
The tid not in clause excludes terms under About Lawyers, Courts & Hearings, and Practice Resources. Only taxonomy terms that are not parent terms are included.  This means only those terms that are the lowest level terms are returned.

.. code-block:: sql

   SELECT tid, name from taxonomy_term_data where tid not in
   (SELECT taxonomy_entity_index.tid from taxonomy_entity_index
   inner join taxonomy_term_data
   on taxonomy_entity_index.tid = taxonomy_term_data.tid
   inner join node on nid = taxonomy_entity_index.entity_id
   inner join field_data_title_field on field_data_title_field.entity_id = nid
   WHERE taxonomy_entity_index.bundle = 'triage_rules'
   AND field_name = 'field_legal_issues' and node.status = 1)
   AND vid = 61 and tid not in
   (517836, 517841, 517846, 524176, 524156, 524161, 524221,
   524171, 524211, 524181, 524236, 524306, 517851, 517856,
   517866, 517896, 517906, 517901, 522986, 517911, 517861,
   517921, 517936, 517926, 517916, 517931, 517941, 517946,
   522646, 517951, 517956, 517966, 517976, 517961, 517971,
   517871, 517876, 517886, 517891, 517881) 
   and tid not in (SELECT parent from taxonomy_term_hierarchy)
   
Queries for Geographic Reach
=============================

Intake Settings
-------------------------------   

Gets all intake settings by zip code when coded by city
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Returns all published intake settings and the associated zip codes.

..  code-block:: sql

   SELECT entity_id as intake_settings_id,
   tid as term_id, 
   name as zip_code
   from field_data_field_cities
   inner join taxonomy_term_data
   where entity_type = 'oas_intake_settings' and tid in
   (SELECT tid from taxonomy_term_hierarchy
     where parent = field_data_field_cities.field_cities_target_id)
     and entity_id in (Select intake_settings_id
                   from oas_intake_settings
                   where enabled = 1)

Gets all intake settings by zip code when coded to counties
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: sql

   SELECT entity_id as intake_settings_id,
   tid as term_id, 
   name as zip_code
   from field_data_field_counties
   inner join taxonomy_term_data
   where entity_type = 'oas_intake_settings' and tid in
   (SELECT tid from taxonomy_term_hierarchy where parent in (
   Select tid from taxonomy_term_hierarchy where parent in (Select tid
   from taxonomy_term_data where tid = field_data_field_counties.field_counties_target_id)))
     and entity_id in (Select intake_settings_id
                   from oas_intake_settings
                   where enabled = 1)
    
Gets all Illinois zip codes; use for statewide intake settings
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


.. code-block:: sql

   Select * from taxonomy_term_hierarchy 
   inner join taxonomy_term_data
   on taxonomy_term_data.tid = taxonomy_term_hierarchy.tid
   WHERE parent in
   (select tid from taxonomy_term_hierarchy
      where parent in
      (select tid from taxonomy_term_hierarchy where parent =
      (Select tid from taxonomy_term_data where name = 'Illinois')))
   and taxonomy_term_data.vid = 76

Intake settings by geographic reach
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: sql

   SELECT intake_settings_id,
   CASE
    WHEN intake_settings_id in (Select entity_id from field_data_field_statewide where entity_type = 'oas_intake_settings' and field_data_field_statewide.field_statewide_value = 1) THEN 'statewide'
    WHEN intake_settings_id in (Select entity_id from field_data_field_cities where entity_type = 'oas_intake_settings') THEN 'city-limited'
    WHEN intake_settings_id in (Select entity_id from field_data_field_counties where entity_type = 'oas_intake_settings') THEN 'county-limited'
   ELSE 'localized'
  END  
  from oas_intake_settings
  
Services
---------

Gets all services by zip code when coded by city
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

..  code-block:: sql

    SELECT  nid,
    node.title as service_title, 
    node.status, 
    tid as term_name, 
    name as zip_code
    from field_data_field_cities
    inner join node on entity_id = nid
    inner join taxonomy_term_data
    where entity_type = 'node'
    and bundle = 'location_services'
    and tid in
    (SELECT tid from taxonomy_term_hierarchy
     where parent = field_data_field_cities.field_cities_target_id)
    and nid in (Select entity_id from oas_intake_settings) 
    order by nid 

Gets all intake settings by zip code when coded to counties
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: sql

   SELECT  nid,
   node.title as service_title, 
   node.status, 
   tid as term_name, 
   name as zip_code
   from field_data_field_counties
   inner join node on entity_id = nid
   inner join taxonomy_term_data
   where entity_type = 'node'
   and bundle = 'location_services'
   and tid in
   (SELECT tid from taxonomy_term_hierarchy where parent in
   (Select tid from taxonomy_term_hierarchy where parent in
   (Select tid from taxonomy_term_data
   where tid = field_data_field_counties.field_counties_target_id)))
   and nid in (Select entity_id from oas_intake_settings)
   order by nid 
   
Gets all services by zip code when coded to zip code
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: sql

   SELECT  nid,
   node.title as service_title, 
   node.status, 
   tid as term_name, 
   name as zip_code
   from field_data_field_zipcodes
   inner join node on entity_id = nid
   inner join taxonomy_term_data
   on field_data_field_zipcodes.field_zipcodes_target_id = tid
   where entity_type = 'node'
   and bundle = 'location_services'
   and nid in (Select entity_id from oas_intake_settings)
   order by nid 
     
Gets all services by zip code when statewide
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: sql

   SELECT  nid,
   node.title as service_title, 
   node.status, 
   tid as term_name, 
   name as zip_code
   from field_data_field_zipcodes
   inner join node on entity_id = nid
   inner join taxonomy_term_data
   on field_data_field_zipcodes.field_zipcodes_target_id = tid
   where entity_type = 'node'
   and bundle = 'location_services'
   and nid in (Select entity_id from oas_intake_settings)
   order by nid 
     
           
     

Gets all Illinois zip codes; use for statewide services
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


.. code-block:: sql

   Select * from taxonomy_term_hierarchy 
   inner join taxonomy_term_data
   on taxonomy_term_data.tid = taxonomy_term_hierarchy.tid
   WHERE parent in
   (select tid from taxonomy_term_hierarchy
      where parent in
      (select tid from taxonomy_term_hierarchy where parent =
      (Select tid from taxonomy_term_data where name = 'Illinois')))
   and taxonomy_term_data.vid = 76

Services with online intake settings by geographic reach
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: sql

   SELECT nid,
   title as service_name,
   status,
   CASE
    WHEN nid in (Select entity_id from field_data_field_statewide 
      where bundle = 'location_services' 
      and field_data_field_statewide.field_statewide_value = 1) 
      THEN 'statewide'
    WHEN nid in (Select entity_id from field_data_field_cities
     where bundle = 'location_services') 
     THEN 'city-limited'
    WHEN nid in (Select entity_id from field_data_field_counties 
    where bundle = 'location_services') 
    THEN 'county-limited'
   ELSE 'zip-codes'
   END
   as geography
   from node
   where type = 'location_services'
   and nid in (Select entity_id from oas_intake_settings)
   ORDER BY nid

  

Triage User 
================

.. code:: sql

   Select 
   triage_id,
   oas_triage_user.uid, 
   age, 
   from_unixtime(oas_triage_user.changed) as triage_changed,
   citizenship, 
   county,
   from_unixtime(oas_triage_user.created) as triage_started, 
   ethnicity, 
   gender,   
   household_adults, 
   household_children,
   household_size, 
   immigrant_status, 
   from_unixtime(intake_created) as intake_started, 
   from_unixtime(intake_changed) as intake_changed,
   last_screen_viewed, 
   marital_status, 
   overincome, 
   primary_language, 
   race,
   state, 
   total_income, 
   total_expenses,
   total_assets, 
   triage_status, 
   zip_code,
   oas_intake_settings.entity_id as 'service', 
   oas_intake_settings.intake_settings_id 
   from oas_triage_user
   left outer join oas_intake_settings 
   on oas_intake_settings.intake_settings_id = oas_triage_user.intake_organization
   where oas_triage_user.uid not in (2, 4486, 4646, 1) 
   and oas_triage_user.created > 1470064821

Triage user legal problem
---------------------------

.. code:: sql

   Select triage_id,
   CASE
   WHEN triage_id in (Select entity_id from field_data_field_triage_problem_history
     where entity_type = 'oas_triage_user' order by delta desc) THEN 
     (Select field_data_field_triage_problem_history.field_triage_problem_history_tid 
      from field_data_field_triage_problem_history where entity_type = 'oas_triage_user' order by delta desc LIMIT 1)
   ELSE
    (Select field_data_field_triage_problem.field_triage_problem_tid from 
     field_data_field_triage_problem where entity_type = 'oas_triage_user' limit 1)
   END   
   as legal_problem,
   CASE
   WHEN triage_id in (Select entity_id from field_data_field_triage_problem_history
    where entity_type = 'oas_triage_user' order by delta desc) THEN 
   (Select name from taxonomy_term_data where tid = (SELECT field_data_field_triage_problem_history.field_triage_problem_history_tid from field_data_field_triage_problem_history where entity_type = 'oas_triage_user' order by delta desc LIMIT 1))
   ELSE
    (Select name from taxonomy_term_data where tid =
    (Select field_data_field_triage_problem.field_triage_problem_tid from 
     field_data_field_triage_problem where entity_type = 'oas_triage_user' limit 1))
   END   
   as legal_problem_name
   from oas_triage_user
   where oas_triage_user.uid not in (2, 4486, 4646, 1) 
   and oas_triage_user.created > 1470064821
   
Triage user population
------------------------
.. code:: sql

   SELECT entity_id as triage_id,
   field_limited_populations_tid as term_id,
   name as population_name,
   FROM field_data_field_limited_populations
   inner join taxonomy_term_data on taxonomy_term_data.tid = field_limited_populations_tid
   where entity_type = 'oas_triage_user'   
   
   
Intake Settings
==================

.. code:: sql

   Select title as service, 
   oas_intake_settings.entity_id as service_id, 
   intake_settings_id, 
   oas_intake_settings.name,
   allow_prisoners,
   apply_asset_limit,
   apply_income_limit,
   callback_number,
   callback_type,
   from_unixtime(oas_intake_settings.changed) as 'last_updated',
   collect_assets,
   collect_citizenship, 
   collect_country,  
   collect_language,
   collect_gender,
   collect_race,
   collect_ethnicity,
   collect_income,
   collect_assets,
   collect_expenses,
   collect_expenses_over_income,
   from_unixtime(oas_intake_settings.created) as 'created',
   enabled,
   intake_limit,
   maximum_allowed_income,  
   maximum_allowed_assets,
   maximum_callbacks_per_slot,
   minimum_minor_age,
   minimum_senior_age,
   personal_exemption_amount,
   CASE
    WHEN intake_limit != 0 THEN
    CASE
      WHEN reset_limit_frequency = 1 THEN 'per day'
      WHEN reset_limit_frequency = 7 THEN 'per week'
      WHEN reset_limit_frequency = 30 THEN 'per month'
    END
    ELSE 'unlimited'
  END 
  as reset_limit_frequency,
  CASE
    WHEN intake_settings_id in 
     (Select entity_id from field_data_field_statewide 
     where entity_type = 'oas_intake_settings' 
     and field_data_field_statewide.field_statewide_value = 1) 
     THEN 'statewide'
    WHEN intake_settings_id in 
     (Select entity_id from field_data_field_cities 
      where entity_type = 'oas_intake_settings') THEN 'city-limited'
    WHEN intake_settings_id in 
     (Select entity_id from field_data_field_counties 
     where entity_type = 'oas_intake_settings') 
     THEN 'county-limited'
   ELSE 'localized'
   END
  as geographic_reach,
  ilao_oas_income_standard.name as 'income_standard'
  from oas_intake_settings
  inner join node on 
  oas_intake_settings.entity_id = nid
  inner join field_data_oas_income_standard ON
  field_data_oas_income_standard.entity_id = intake_settings_id
  inner join ilao_oas_income_standard on 
  ilao_oas_income_standard.id = 
  field_data_oas_income_standard.oas_income_standard_target_id
  
Intake settings & Legal issues
-------------------------------

.. code:: sql
   
   SELECT DISTINCT intake_settings_id,
   oas_intake_settings.name as intake_settings_name,
   enabled,
   from_unixtime(created) as 'created', 
   from_unixtime(changed) as 'last_updated',
   tid as term_id,
   taxonomy_term_data.name as legal_issue
   from oas_intake_settings
   inner join field_data_field_legal_issues on field_data_field_legal_issues.entity_id = intake_settings_id
   inner join taxonomy_term_data on field_data_field_legal_issues.field_legal_issues_tid = tid
   where field_data_field_legal_issues.entity_type = 'oas_intake_settings'
   and tid not in (Select parent from taxonomy_term_hierarchy where taxonomy_term_hierarchy.tid = tid)
   order by intake_settings_id
 

Financial Categories per Intake Setting
----------------------------------------

Assets
^^^^^^^^

.. code:: sql

   SELECT field_data_oas_asset_categories.entity_id as intake_settings_id, 
   oas_intake_settings.name as intake_settings_name, 
   oas_asset_categories_target_id as financial_id, 
   ilao_oas_financial_category.name as asset,
   subcategory 
   FROM field_data_oas_asset_categories 
   inner join oas_intake_settings 
   on oas_intake_settings.intake_settings_id = 
    field_data_oas_asset_categories.entity_id 
   inner join ilao_oas_financial_category 
   on ilao_oas_financial_category.ilao_oas_financial_category_id = 
   oas_asset_categories_target_id 
   order by oas_intake_settings.intake_settings_id, delta

Income
^^^^^^^^

.. code:: sql

   SELECT field_data_oas_income_categories.entity_id as intake_settings_id, 
   oas_intake_settings.name as intake_settings_name, 
   oas_income_categories_target_id as financial_id, 
   ilao_oas_financial_category.name as income,
   subcategory
   FROM field_data_oas_income_categories 
   inner join oas_intake_settings 
   on oas_intake_settings.intake_settings_id = 
    field_data_oas_income_categories.entity_id 
   inner join ilao_oas_financial_category 
   on ilao_oas_financial_category.ilao_oas_financial_category_id = 
   oas_income_categories_target_id 
   order by oas_intake_settings.intake_settings_id, delta
   
Expenses
^^^^^^^^^

.. code:: sql

   SELECT field_data_oas_expense_categories.entity_id as intake_settings_id, 
   oas_intake_settings.name as intake_settings_name, 
   oas_expense_categories_target_id as financial_id, 
   ilao_oas_financial_category.name as income,
   subcategory
   FROM field_data_oas_expense_categories 
   inner join oas_intake_settings 
   on oas_intake_settings.intake_settings_id = 
    field_data_oas_expense_categories.entity_id 
   inner join ilao_oas_financial_category 
   on ilao_oas_financial_category.ilao_oas_financial_category_id = 
   oas_expense_categories_target_id 
   order by oas_intake_settings.intake_settings_id, delta
   
Intake settings: waive Income based on population
--------------------------------------------------   

.. code:: sql

   SELECT field_data_oas_income_exempt.entity_id as intake_settings_id,
   oas_income_exempt_tid as term_id,
   taxonomy_term_data.name as population,
   oas_intake_settings.enabled as status
   from field_data_oas_income_exempt
   inner join taxonomy_term_data on tid = oas_income_exempt_tid
   inner join oas_intake_settings on intake_settings_id = field_data_oas_income_exempt.entity_id
   where field_data_oas_income_exempt.entity_type = 'oas_intake_settings'
   
Triage Rules
================

List of triage rules
---------------------

.. code:: sql

   SELECT DISTINCT nid,
   field_data_title_field.title_field_value as title,
   status, 
   from_unixtime(created) as created, 
   from_unixtime(changed) as last_updated 
   from node 
   inner join field_data_title_field on nid = field_data_title_field.entity_id
   where type = 'triage_rules'

Triage rules with associated services
--------------------------------------

.. code:: sql

   SELECT DISTINCT node.nid,
   field_data_title_field.title_field_value as title,
   node.status, 
   from_unixtime(node.created) as 'created', 
   from_unixtime(node.changed) as 'last_updated',
   field_data_field_service.field_service_target_id as 'service_id',
   services.title as 'service_title'
   from node 
   inner join field_data_title_field on nid = field_data_title_field.entity_id
   inner join field_data_field_service ON
   field_data_field_service.entity_id = nid
   inner join node as services ON
   services.nid = field_data_field_service.field_service_target_id
   where node.type = 'triage_rules'
   
Triage rules with associated legal issues
-------------------------------------------

Mapping of legal issues tagged to a triage rules item.  Note that this query excludes anything but lowest level terms because legal issue must be an exact match.
 

.. code:: sql

   SELECT DISTINCT node.nid,
   field_data_title_field.title_field_value as title,
   node.status, 
   from_unixtime(node.created) as 'created', 
   from_unixtime(node.changed) as 'last_updated',
   tid as term_id,
   name as legal_issue
   from node 
   inner join field_data_title_field on nid = field_data_title_field.entity_id
   inner join field_data_field_legal_issues on field_data_field_legal_issues.entity_id = nid
   inner join taxonomy_term_data on field_data_field_legal_issues.field_legal_issues_tid = tid
   where node.type = 'triage_rules'
   and field_data_field_legal_issues.bundle = 'triage_rules'
   and tid not in (Select parent from taxonomy_term_hierarchy where taxonomy_term_hierarchy.tid = tid)
   order by nid
   
Triage rules webform components
---------------------------------

.. code:: sql

   SELECT node.nid as triage_rules_id,
   cid as component_id, 
   form_key,
   name,
   webform_component.type,
   value,
   status
   FROM webform_component
   inner join node on node.nid = webform_component.nid
   where node.type = 'triage_rules'
   order by node.nid, cid

Triage rules webform conditionals
----------------------------------

.. code:: sql

   SELECT webform_conditional_rules.nid, 
   webform_conditional_rules.rgid, 
   rid, 
   source_type, 
   source, 
   operator, 
   value, 
   action, 
   argument 
   FROM webform_conditional_rules
   inner join webform_conditional_actions
   on webform_conditional_rules.rgid = webform_conditional_actions.rgid
   order by webform_conditional_rules.nid, webform_conditional_rules.rgid   

Services with Online Intake
============================

List of services that have at least one intake settings
-------------------------------------------------------

.. code:: sql

   Select node.nid as service_id,
   node.title as service_title,
   node.status,
   field_data_field_location_ref.field_location_ref_target_id as location_id,
   locations.title as 'location',
   og_membership.gid as organization_id,
   organizations.title as organization_name
   from node 
   inner join field_data_field_location_ref on field_data_field_location_ref.entity_id = node.nid 
   inner join og_membership on etid = node.nid
   inner join node as locations
   on locations.nid = field_data_field_location_ref.field_location_ref_target_id
   inner join node as organizations
   on og_membership.gid = organizations.nid
   where node.type = 'location_services'
   and node.nid in (Select entity_id from oas_intake_settings)
   
List of services with limited populations served
-------------------------------------------------

.. code:: sql
   
   Select node.nid as service_id,
   node.title as service_title,
   status,
   field_data_field_limited_populations.field_limited_populations_tid as term_id,
   name as population
   from node 
   inner join field_data_field_limited_populations on entity_id = node.nid
   inner join taxonomy_term_data on tid = field_data_field_limited_populations.field_limited_populations_tid
   where node.type = 'location_services'
   and node.nid in (Select entity_id from oas_intake_settings)
   and bundle = 'location_services'


List of services with lowest-level legal issues
--------------------------------------------------
We use only the lowest level since that's what users are forced to drill down to.

.. code:: sql

   Select node.nid as service_id,
   node.title as service_title,
   node.status,
   tid as term_id,
   name as legal_issue
   
   from node 
   inner join field_data_field_legal_issues
   on node.nid = field_data_field_legal_issues.entity_id
   inner join taxonomy_term_data
   on taxonomy_term_data.tid = field_data_field_legal_issues.field_legal_issues_tid
   
   where node.type = 'location_services'
   and node.nid in (Select entity_id from oas_intake_settings)
   and bundle = 'location_services'
    and tid not in (Select parent from taxonomy_term_hierarchy
                  )
   
Services by geographic coverage
---------------------------------


Services vs intake settings geographic coverage
------------------------------------------------

.. code:: sql

   Select oas_intake_settings.name as intake_settings, 
   field_data_field_same_service_area_as_locat.entity_id as intake_settings_id, 
   oas_intake_settings.enabled as status, 
   field_data_field_same_service_area_as_locat.field_same_service_area_as_locat_value 
    as has_same_geographic_area, 
   oas_intake_settings.entity_id as service_id, 
   node.title as service_name, 
   node.status as service_status 
   from field_data_field_same_service_area_as_locat 
   inner join oas_intake_settings 
   on intake_settings_id = field_data_field_same_service_area_as_locat.entity_id 
   inner join node 
   on oas_intake_settings.entity_id = node.nid 
   where field_data_field_same_service_area_as_locat.entity_type = 'oas_intake_settings'

Services by eligibility type
-----------------------------

.. code:: sql

   Select entity_id as service_id , title as service_name, status,
   CASE
    WHEN field_service_eligibility_value = 1
     THEN 'Free to everyone'
    WHEN field_service_eligibility_value = 2
     THEN 'Free to eligible persons'
    WHEN field_service_eligibility_value = 3
     THEN 'Sliding scale'
    WHEN field_service_eligibility_value = 4
     THEN 'Flat fee'
  END
  as income_eligibility
  from field_data_field_service_eligibility
  inner join node on entity_id = nid
  where bundle = 'location_services'
  and nid in (Select entity_id from oas_intake_settings)     
  
Legal issues combined table
----------------------------

.. code-block:: sql

   Select triage_id, intake_status,
   field_data_field_triage_problem.field_triage_problem_tid,
   taxonomy_term_data.name,
   (Select name from taxonomy_term_data where tid =
   (Select field_data_field_triage_problem_history.field_triage_problem_history_tid
   from field_data_field_triage_problem_history
   where entity_id = triage_id
   and entity_type = 'oas_triage_user'
   order by delta DESC limit 1)
   limit 1)
   as history
   from oas_triage_user
   left join field_data_field_triage_problem on
   field_data_field_triage_problem.entity_id = triage_id
   inner join taxonomy_term_data on tid = 
   field_data_field_triage_problem.field_triage_problem_tid
   order by triage_id DESC  
   
Help type requested by triage user
-----------------------------------

.. code-block:: sql

  SELECT triage_id,
  GROUP_CONCAT(field_data_oas_triage_help_type.oas_triage_help_type_value)
  from oas_triage_user
  inner join field_data_oas_triage_help_type on triage_id = entity_id
  group by triage_id   

   
   
