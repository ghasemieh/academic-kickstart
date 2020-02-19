+++
# Slider widget.
widget = "slider"  # See https://sourcethemes.com/academic/docs/page-builder/
headless = true  # This file represents a page section.
active = true  # Activate this widget? true/false
weight = 1  # Order that this section will appear.

# Slide interval.
# Use `false` to disable animation or enter a time in ms, e.g. `5000` (5s).
interval = false

# Slide height (optional).
# E.g. `500px` for 500 pixels or `calc(100vh - 70px)` for full screen.
height = ""

# Slides.
# Duplicate an `[[item]]` block to add more slides.
[[item]]
  title = "Data Science Fellow"
  content = ""
  align = "center"  # Choose `center`, `left`, or `right`.

  # Overlay a color or image (optional).
  #   Deactivate an option by commenting out the line, prefixing it with `#`.
  overlay_color = "#666"  # An HTML color value.
  overlay_img = "sm.png"  # Image path relative to your `static/img/` folder.
  overlay_filter = 0.2  # Darken the image. Value in range 0-1.

  # Call to action button (optional).
  #   Activate the button by specifying a URL and button label below.
  #   Deactivate by commenting out parameters, prefixing lines with `#`.
  cta_label = "Sharpest Minds"
  cta_url = "https://sharpestminds.com/"
  cta_icon_pack = "fas"
  cta_icon = "graduation-cap"

[[item]]
  title = "Research Associate"
  content = ""
  align = "center"

  overlay_color = "#000"  # An HTML color value.
  overlay_img = "aisc.png"  # Image path relative to your `static/img/` folder.
  overlay_filter = 0.0  # Darken the image. Value in range 0-1.

  cta_label = "A.I.S.C"
  cta_url = "https://aisc.ai.science/"
  cta_icon_pack = "fas"
  cta_icon = "graduation-cap"
  
[[item]]
  title = "Bugzilla Contributor"
  content = "The Mozilla Bug Repository"
  align = "center"

  overlay_color = "#333"  # An HTML color value.
  overlay_img = "Bugzilla.png" # Image path relative to your `static/img/` folder.
  overlay_filter = 0.1  # Darken the image. Value in range 0-1.
  
  cta_label = "Bugzilla"
  cta_url = "https://bugzilla.mozilla.org/"
+++
