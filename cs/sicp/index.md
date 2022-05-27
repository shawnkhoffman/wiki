---
title: SICP Wiki
authors: Shawn Hoffman
editors: 
summary: "These study notes are from Structure and Interpretation of Computer Programs - 2nd Edition (MIT Electrical Engineering and Computer Science) by Abelson, H. and Sussman, G."
tags: [SICP, Computer Science]
keywords: SICP, Computer Science
references: https://web.mit.edu/6.001/6.037/sicp.pdf, https://download.racket-lang.org/, https://archive.org/details/ucberkeley-webcast-PL3E89002AA9B9879E, https://youtube.com/playlist?list=PLu-l3j4eka05K_tXNEVaOGy1Vb-oO0Y2s
toc: false

folder: /wiki/cs/sicp/
sidebar: compsci_sicp_sidebar
---

## Intro

This can be a difficult book to follow if you have no background in programming, though having such a background is not required. What I provide here are my study notes and interpretations of the subject matter provided in this book. I hope that this helps you to better absorb it.

Furthermore, this book uses the Scheme dialect of the Lisp programming language, which is available for most common platforms. To follow along and to do the lessons in the book, I recommend the Racket IDE which includes DrScheme – to install it, you can find a link at the bottom of this page.

Moreover, Scheme is a pedagogical language and contains only the essentials so that you can learn programming in more advanced languages later. It is highly recommended that you follow the original SICP book that uses Scheme rather than other iterations, even if you never write another line of Scheme again.

## Lectures

I provide links to two different sets of lectures here. The first one is the one most recommended for this book: the CS 61A course offered by Dr. Brian Harvey at the University of California, Berkeley in California during Spring Semester of 2011. The second one is to the COMP 3007 course offered by Dr. Andrew Runka at Carleton University in Canada during Fall Semester of 2020. I encourage you to watch both.

---

## Chapters

<html>
{% if site.output == "pdf" %}
{{site.data.alerts.note}} The content on this page doesn't display well on PDF.  {{site.data.alerts.end}}
{% endif %}

{% unless site.output == "pdf" %}
<script src="/js/jquery.shuffle.min.js"></script>
<script src="/js/jquery.ba-throttle-debounce.min.js"></script>
{% endunless %}

      <div class="filter-options">
      <button class="btn btn-primary" data-group="all">All</button>
      <button class="btn btn-primary" data-group="chapter-1">Chapter 1</button>
      <!-- <button class="btn btn-primary" data-group="chapter-2">Chapter 2</button> -->
    </div>      

<div id="grid" class="row">


    <div class="col-xs-6 col-sm-4 col-md-4" data-groups='["chapter-1"]'>

               <div class="panel panel-default">
               <div class="panel-heading">Chapter 1</div>
               <div class="panel-body">
                 Building Abstractions with Procedures
                <ul>
                  <li><a href="ch1/">See more</a></li>
                </ul>
               </div>
            </div>
    
    </div>
   

    <!-- <div class="col-xs-6 col-sm-4 col-md-4" data-groups='["chapter-2"]'>

        <div class="panel panel-default">
            <div class="panel-heading">Chapter 2</div>
            <div class="panel-body">
              The smallest deployable units of computing that you can create and manage in Kubernetes.
                <ul>
                    {% assign counter = '0' %}
                    {% for page in site.pages %}
                      {% for tag in page.tags %}
                        {% if tag == "Pods" and counter < '3' %}
                          {% capture counter %}{{ counter | plus:'1' }}{% endcapture %}
                          <li><a href="{{page.url}}">{{page.title}}</a></li>
                        {% endif %}
                      {% endfor %}
                    {% endfor %}
                </ul>
                <ul>
                  <li><a href="/content-pods">See more</a></li>
                </ul>
            </div>
        </div>
        
    </div> -->
          <!-- sizer -->
      <div class="col-xs-6 col-sm-4 col-md-1 shuffle_sizer"></div>          


    </div><!-- /#grid -->

{% unless site.output == "pdf" %}
{% include initialize_shuffle.html %}
{% endunless %}
</html>

{{site.data.alerts.note}} There is more coming to each subject in this wiki category as more content is added.{{site.data.alerts.end}}
