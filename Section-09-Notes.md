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
  
    **Event Side**
    ```php
    echo "<hr class='section-break'>";
    echo "<h2 class='headline headline--medium'>Related Program(s)</h2>";
    echo "<ul class='link-list min-list'>";

    foreach($relatedPrograms as $program) { ?>

      <li><a href="<?php echo get_the_permalink($program); ?>"><?php echo get_the_title($program); ?></a></li>

      <?php
    }

    echo "</ul>";
    ```
    
    **Program Side**
    
    ```php
    <!-- Just need to add on ONE extra filter -->
    <?php 
      $today = date('Ymd');

      // Custom Query with Ordering and Sorting
      $homepageEvents = new WP_Query(
        array(
          'posts_per_page' => -1, // 2
          'post_type'      => 'event',
          'meta_key'       => 'event_date',
          'orderby'        => 'meta_value_num', // formerly 'post_date', 'rand', meta_value !event_date
          'order'          => 'ASC',
          'meta_query'     => array( // eliminate non-adherents to sub-query
            array(
              'key' => 'event_date',
              'compare' => '>=',
              'value' => $today,
              'type' => 'numeric'
            ),
            array(
              'key' => 'related_programs', // Need to serialize the string of text
              'compare' => 'LIKE',
              'value' => '"' . get_the_id() . '"'
            )
          )
        )
      );

      if ($homepageEvents->have_posts()) {
        echo "<hr class='section-break'>";
        echo "<h2 class='headline headline--medium'>Upcoming " . get_the_title() ." Events</h2>";

        while($homepageEvents->have_posts()) {
          $homepageEvents->the_post(); ?>

          <div class="event-summary">
            <a class="event-summary__date t-center" href="<?php the_permalink(); ?>">
              <span class="event-summary__month">
                <!-- <?php the_field('event_date'); ?></span> --><!-- CPT Date -->
                <?php 
                  $eventDate = new DateTime(get_field('event_date')); // the_field >> get_field
                  echo $eventDate->format('M');
                ?>
              <span class="event-summary__day"><?php echo $eventDate->format('d'); ?></span>
            </a>
            <div class="event-summary__content">
              <h5 class="event-summary__title headline headline--tiny">
                <a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></h5>
              <p>
                <!-- <?php echo wp_trim_words(get_the_content(), 18); ?> -->
                <!-- Nuanced Excerpt -->
                <?php 
                  if (has_excerpt()) {
                    echo get_the_excerpt();
                  } else {
                    echo wp_trim_words(get_the_content(), 18);
                  }
                ?>
                <a href="<?php the_permalink(); ?>" class="nu gray">Learn more</a>
              </p>
            </div>
          </div>
        <?php 
        }  
      }
    ?>
    ```
    
  - [ ] 39. Quick Program Edits 9min
