/* =====================================================
   NOTICE BAR CPT
===================================================== */
function drn_register_notice_bar_cpt() {

    register_post_type('notice_bar', array(
        'labels' => array(
            'name' => 'Notice Bars',
            'singular_name' => 'Notice Bar'
        ),
        'public' => false,
        'show_ui' => true,
        'menu_icon' => 'dashicons-megaphone',
        'supports' => array('title'),
    ));

}
add_action('init', 'drn_register_notice_bar_cpt');


/* =====================================================
   META BOX
===================================================== */
function drn_notice_bar_meta_box() {
    add_meta_box(
        'drn_notice_bar_fields',
        'Notice Bar Settings',
        'drn_notice_bar_meta_box_callback',
        'notice_bar',
        'normal',
        'high'
    );
}
add_action('add_meta_boxes', 'drn_notice_bar_meta_box');


function drn_notice_bar_meta_box_callback($post) {

    wp_nonce_field('drn_notice_save', 'drn_notice_nonce');

    $enabled     = get_post_meta($post->ID, '_drn_enabled', true);
    $message     = get_post_meta($post->ID, '_drn_message', true);
    $link        = get_post_meta($post->ID, '_drn_link', true);
    $label       = get_post_meta($post->ID, '_drn_label', true);
    $bg          = get_post_meta($post->ID, '_drn_bg', true);
    $text        = get_post_meta($post->ID, '_drn_text', true);
    $label_color = get_post_meta($post->ID, '_drn_label_color', true);
    $icon        = get_post_meta($post->ID, '_drn_icon', true);
?>

<p><strong>Enable Notice</strong><br>
<input type="checkbox" name="drn_enabled" value="1" <?php checked($enabled, 1); ?>></p>

<p><strong>Notice Message</strong><br>
<textarea name="drn_message" style="width:100%;"><?php echo esc_textarea($message); ?></textarea></p>

<p><strong>Notice Link</strong><br>
<input type="text" name="drn_link" value="<?php echo esc_attr($link); ?>" style="width:100%;"></p>

<p><strong>Link Label</strong><br>
<input type="text" name="drn_label" value="<?php echo esc_attr($label); ?>" placeholder="Learn More"></p>

<p><strong>Background Color</strong><br>
<input type="color" name="drn_bg" value="<?php echo esc_attr($bg ?: '#f5b335'); ?>"></p>

<p><strong>Text Color</strong><br>
<input type="color" name="drn_text" value="<?php echo esc_attr($text ?: '#000000'); ?>"></p>

<p><strong>Label Color (Optional)</strong><br>
<input type="color" name="drn_label_color" value="<?php echo esc_attr($label_color); ?>"></p>

<p><strong>Learn More Icon</strong><br>
<input type="text" name="drn_icon" value="<?php echo esc_attr($icon ?: '→'); ?>" placeholder="→"></p>

<?php
}


/* =====================================================
   SAVE META
===================================================== */
function drn_save_notice_bar_meta($post_id) {

    if (!isset($_POST['drn_notice_nonce'])) return;
    if (!wp_verify_nonce($_POST['drn_notice_nonce'], 'drn_notice_save')) return;
    if (defined('DOING_AUTOSAVE') && DOING_AUTOSAVE) return;
    if (wp_is_post_revision($post_id)) return;
    if (get_post_type($post_id) != 'notice_bar') return;

    update_post_meta($post_id, '_drn_enabled', isset($_POST['drn_enabled']) ? 1 : 0);

    if (isset($_POST['drn_message']))
        update_post_meta($post_id, '_drn_message', sanitize_textarea_field($_POST['drn_message']));

    if (isset($_POST['drn_link']))
        update_post_meta($post_id, '_drn_link', esc_url_raw($_POST['drn_link']));

    if (isset($_POST['drn_label']))
        update_post_meta($post_id, '_drn_label', sanitize_text_field($_POST['drn_label']));

    if (isset($_POST['drn_bg']))
        update_post_meta($post_id, '_drn_bg', sanitize_hex_color($_POST['drn_bg']));

    if (isset($_POST['drn_text']))
        update_post_meta($post_id, '_drn_text', sanitize_hex_color($_POST['drn_text']));

    if (isset($_POST['drn_label_color']))
        update_post_meta($post_id, '_drn_label_color', sanitize_hex_color($_POST['drn_label_color']));

    if (isset($_POST['drn_icon']))
        update_post_meta($post_id, '_drn_icon', sanitize_text_field($_POST['drn_icon']));
}
add_action('save_post_notice_bar', 'drn_save_notice_bar_meta');


/* =====================================================
   FRONTEND DISPLAY (ROTATIONAL)
===================================================== */
function drn_display_top_notice_bar() {

    $args = array(
        'post_type' => 'notice_bar',
        'post_status' => 'publish',
        'meta_key' => '_drn_enabled',
        'meta_value' => 1,
        'posts_per_page' => -1
    );

    $query = new WP_Query($args);

    if ($query->have_posts()) :

        echo '<div class="drn-notice-slider">';

        while ($query->have_posts()) : $query->the_post();

            $message = get_post_meta(get_the_ID(), '_drn_message', true);
            $link    = get_post_meta(get_the_ID(), '_drn_link', true);
            $label   = get_post_meta(get_the_ID(), '_drn_label', true);
            $bg      = get_post_meta(get_the_ID(), '_drn_bg', true);
            $text    = get_post_meta(get_the_ID(), '_drn_text', true);
            $label_color = get_post_meta(get_the_ID(), '_drn_label_color', true);
            $icon = get_post_meta(get_the_ID(), '_drn_icon', true);

            if(empty($label_color)) $label_color = $text;
            if(empty($icon)) $icon = '→';
?>

<div class="drn-notice-slide" style="background:<?php echo esc_attr($bg ?: '#f5b335'); ?>; color:<?php echo esc_attr($text ?: '#000'); ?>;">
    <span class="drn-notice-text"><?php echo esc_html($message); ?></span>

    <?php if($link && $label): ?>
        <a href="<?php echo esc_url($link); ?>" class="drn-notice-btn" style="color:<?php echo esc_attr($label_color); ?>">
            <?php echo esc_html($label); ?> <?php echo esc_html($icon); ?>
        </a>
    <?php endif; ?>
</div>

<?php endwhile;

echo '</div>';

wp_reset_postdata();
endif;
}
add_action('wp_body_open', 'drn_display_top_notice_bar');


/* =====================================================
   INLINE CSS
===================================================== */
add_action('wp_head', function() {
?>
<style>
.drn-notice-slider { position:relative; overflow:hidden; }
.drn-notice-slide { display:none; text-align:center; padding:10px; font-size:14px; }
.drn-notice-slide.active { display:flex; justify-content:center; gap:15px; }
.drn-notice-btn { font-size:13px; font-weight:600; text-decoration:none; border-bottom:1px solid currentColor; }
</style>
<?php });


/* =====================================================
   INLINE JS (ROTATION)
===================================================== */
add_action('wp_footer', function() {
?>
<script>
document.addEventListener("DOMContentLoaded", function () {
    const slides = document.querySelectorAll('.drn-notice-slide');
    let index = 0;
    if(!slides.length) return;
    slides[0].classList.add('active');

    setInterval(()=>{
        slides[index].classList.remove('active');
        index = (index + 1) % slides.length;
        slides[index].classList.add('active');
    }, 4000);
});
</script>
<?php });
?>
<style>
/* ===== NOTICE WRAPPER ===== */
.drn-notice-slider {
    position: relative;
    width: 100%;
    z-index: 9999;
}

/* ===== SINGLE NOTICE ===== */
.drn-notice-slide {
    display: none;
    text-align: center;
    padding: 10px 20px;
    font-size: 15px;
    font-weight: 500;
    line-height: 1.4;
    transition: all .4s ease;
}

/* Active Notice */
.drn-notice-slide.active {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 15px;
}

/* ===== TEXT ===== */
.drn-notice-text {
    display: inline-block;
}

/* ===== LINK BUTTON ===== */
.drn-notice-btn {
    display: inline-flex;
    align-items: center;
    gap: 5px;
    font-size: 14px;
    font-weight: 600;
    text-decoration: none;
    border-bottom: 1px solid currentColor;
    transition: all .3s ease;
}

/* Hover Effect */
.drn-notice-btn:hover {
    opacity: .7;
}

/* ===== MOBILE ===== */
@media(max-width:768px){

    .drn-notice-slide.active {
        flex-direction: column;
        gap: 6px;
        padding: 12px 15px;
        font-size: 14px;
    }

    .drn-notice-btn {
        font-size: 13px;
    }

}

.drn-notice-slide {
    opacity: 0;
    transition: opacity .5s ease;
    position: absolute;
    width: 100%;
}

.drn-notice-slide.active {
    opacity: 1;
    position: relative;
}
</style>

<script>
document.addEventListener("DOMContentLoaded", function () {

    const slides = document.querySelectorAll('.drn-notice-slide');
    let current = 0;
    let interval;

    if (!slides.length) return;

    // Show first notice
    slides[current].classList.add('active');

    function showNextSlide() {
        slides[current].classList.remove('active');
        current = (current + 1) % slides.length;
        slides[current].classList.add('active');
    }

    function startRotation() {
        interval = setInterval(showNextSlide, 4000);
    }

    function stopRotation() {
        clearInterval(interval);
    }

    // Start auto rotation
    startRotation();

    // Pause on hover (UX upgrade)
    const slider = document.querySelector('.drn-notice-slider');

    if (slider) {
        slider.addEventListener('mouseenter', stopRotation);
        slider.addEventListener('mouseleave', startRotation);
    }

});
</script>
<?php
