---
layout: none
---
{%- assign guide = site.posts.first -%}
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>{{ site.title }}</title>
  <meta http-equiv="refresh" content="0; url={{ guide.url | relative_url }}">
  <link rel="canonical" href="{{ guide.url | relative_url }}">
</head>
<body>
  <p><a href="{{ guide.url | relative_url }}">Open the guide</a></p>
</body>
</html>
