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
    
  - [ ] 38. Displaying Relationships (Front-End) 20min
  - [ ] 39. Quick Program Edits 9min
