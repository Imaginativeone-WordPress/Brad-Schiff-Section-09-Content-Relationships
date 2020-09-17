- [ ] Section 9: Programs Post Type 3/3 | 47min
  - [ ] 37. Creating Relationships Between Content 19min
    - Create "Program" Post Type
    - single-program.php
    - archive-program.php
    - Adjust default query in functions.php
    
    ```php
    if (!is_admin() AND is_post_type_archive('program') AND $query->is_main_query()) {
      $query->set('posts_per_page', -1);
      $query->set('orderby',  'title');
      $query->set('order',    'ASC');
    }
    ```
    - Create a relationship between the Biology Program and the Event "The Science of Cats"
    - Need a custom (relationship) field
    - WP Admin > Custom Fields
      - Add a New Field Group
        - "Related Program"
          - Field Label: Related Program(s)
          - Field Name: related_programs
          - Field Type: Relational > Relationship
            - Instructions: blank
            - Required: No
            - Filter by Post Type: Program (Posts)(Very Important)
            - Filter by Taxonomy: No change (All Taxonomies)
            - Filters: Yes-Search, No-Post Type, No-Taxonomy
          - Location (Section)
            - Show this field group if:
              - Post Type [is equal to] "Event"
    - Publish
    - Verify Relationship
      - "The Science of Cats" Event
      - Mistakes I've Made: I didn't select Filter by Post Type: "Program"
    - How do I use that custom field (relationship) value on the front end of the web site?
  - [ ] 38. Displaying Relationships (Front-End) 20min
  - [ ] 39. Quick Program Edits 9min
