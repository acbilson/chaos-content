+++
IsHome = true
kind = "home"
title = ""
+++
<style>

div.home-page {
  display: flex;
  flex-flow: column nowrap;
}

div.title-body {
  margin-left: 2%;

}

.title-img-container {
    position: relative;
    width: 70%;
    height: auto;
    margin: auto;
}

.title-img {
    border-radius: 50%;
}

/* maximized screen */
@media screen and (min-width: 80em) {

div.home-page {
  display: flex;
  flex-flow: row nowrap;
}
}


</style>

<div class="home-page">
  <div class="title-img-container">
    <img class="title-img" src="https://30odha.by.files.1drv.com/y4mKYu6Uh5IYc_-I2yvtnVVEfXY4lJGm960ILB0GLEYamEMHduu_C4BgCJeP3yBc6OjrU8-Stml9OB7kLSdBTpcmuVtmGL-7TdUOSgSCjvAHb6Fs0eMpSYjoHBvK_YT7qTQjwFiWimA_8hb-Is5zNRnfelGBXsya1K-OLg_rkzOw3L2eLQ9ff92PMD0D-aq8dAQjCreVni7aX3vUFO1-Y5R8Q?width=495&height=660&cropmode=none" alt="Graham on Alex's Shoulders">
  </div>

  <div class="title-body">
    <p>Welcome! You can call me Alex.<p>
    <p>If you're here to work together, check out my <a href="#card">card</a>.</p>
    <p>If you're here to read my writing, scroll down to peruse my latest work or see it all <a href="{{< ref "/posts" >}}">here</a>.</p>
    <p>I write about a wide range of interests. You can peruse all my work below or filter by the following topics to see both posts and comments:</p>
    <div style="text-align: center;">
    <a href="{{< ref "/categories/business" >}}">Business</a> &bull;
    <a href="{{< ref "/categories/technology" >}}">Technology</a> &bull;
    <a href="{{< ref "/categories/personal" >}}">Personal</a> &bull;
    <a href="{{< ref "/categories/spiritual" >}}">Spiritual</a> &bull;
    <a href="{{< ref "/categories/resources" >}}">Resources</a>
    <div>
    <p>If you want to see what I'm reading, thinking or doing, check out my <a href="{{< ref "/comments" >}}">comments</a>.</p>
    <p>If you'd like to see other independent writers I follow, see my <a href="{{< ref "/blogroll/_index.md" >}}">blog roll</a>.</p>
  </div>
</div>

