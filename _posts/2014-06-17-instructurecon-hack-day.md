---
layout: post
title: InstructureCon Hack Day
date: '2014-06-17 19:40:05 -0700'
categories:
- Ruby on Rails
tags:
- canvas
- instructure
- lti-integration
---
<p><span style="color: #c0c0c0;"><em>The opinions or statements expressed herein should not be taken as a position of or endorsement by the University of California, Berkeley.</em></span></p>
<p>I'm currently at InstructureCon attending the "Hack Day" event, which is simply an event where any developers wishing to integrate their systems with Canvas can ask questions, talk to Canvas developers, etc.</p>
<p>Here are some things I've clarified with their developers thus far, thanks to Eric Berry and Brian Palmer (<a href="https://github.com/codekitchen">codekitchen</a>).</p>
<h2>LTI Template Builder</h2><br />
Eric Berry (<a href="https://github.com/cavneb">cavneb</a>) is part of the Developer Support team at Instructure, which is a group that develops guides and tools to help developers integrate their systems with Canvas. He informed me of the <a href="http://lti-template-builder.herokuapp.com/" target="_blank">LTI Template Builder</a>, which provides a command you can use to generate a Ruby on Rails engine that provides a template for the LTI application type of your choice.</p>
<h2>Canvas Plugins</h2><br />
In the <a href="https://github.com/instructure/canvas-lms/wiki/Coding-Guidelines#enhancements-and-extensions">Coding Guidelines</a> documentation for Canvas-LMS, the possibility of developing a plugin for Canvas is mentioned. The documentation mentions &ldquo;Plugins can be registered at runtime but only appear in the interface for enabled root accounts&rdquo;. I had assumed this meant that plugins could be enabled for a primary account (our account), but this was a wrong assumption. This only means that Site Admins for a Canvas instance, such as Canvas employees that are the root Administrators for the Cloud hosted Canvas service, are able to manage / activate these plugins. For Canvas to introduce such a plugin to their cloud hosted service, the plugin would need to provide functionality that benefits all institutions, without conflicts.</p>
<p>For instance, the Adobe Connect plugin for web conferencing was developed by <a href="http://www.ocadu.ca/">OCAD University</a>. Any proposed plugins that are developed would need to meet similar criteria of use across institutions.</p>
<h2>Custom Javascript and CSS Application Logic</h2><br />
With the cloud hosted Canvas service, the custom Javascript and CSS files specified under your account settings are applied to the pages ONLY when a hostname associated with your account is in use. For instance, UC Berkeley uses <a href="http://bcourses.berkeley.edu/" target="_blank">http://bcourses.berkeley.edu</a> with the Canvas Cloud hosted service.</p>
<p>When using a local Canvas instance, you'll likely use <a href="http://localhost:3000/" target="_blank">http://localhost:3000/</a>, and thus the Javascript and CSS from an account may be applied to other accounts that are not configured with a custom Javascript and CSS configuration.</p>
<h2>Canvas Refresh Interval</h2><br />
Canvas refreshes the configuration/data for their Beta and Test systems on a specific schedule. I got some details on what this schedule is:</p>
<ul>
<li>Beta - Refresh every Sunday afternoon</li>
<li>Test - Refresh every 3 weeks on Sunday afternoon</li><br />
</ul></p>
<h2>Custom Javascript and CSS API</h2><br />
Every time the Beta and Test Canvas instances are updated, we have to manually update the Javascript and CSS configuration for our account so that they point to the Development and QA instances of our LTI application server. Luckily I was just informed that an API is coming soon that will make it possible to update these URLs.</p>
<p>Also a new option will soon be supported to store the Javascript and CSS code within Canvas, instead of pointing to an external URL that may go offline.</p>
<h2>Using RBEnv or RVM with Canvas</h2><br />
I noted to Brian Palmer that when I specify a .ruby-version or .ruby-gemset file with my local Canvas instance, the files show up expecting to be staged in the Git repository. I noted that these files aren't configured in the .gitignore file in the Canvas-LMS repository.</p>
<p>He informed me that this intentional. Canvas developers are expected to configure these files as <a href="https://help.github.com/articles/ignoring-files#create-a-global-gitignore" target="_blank">ignored globally for Git</a>.</p>
