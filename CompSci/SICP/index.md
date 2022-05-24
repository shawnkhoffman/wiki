---
title: SICP Wiki
summary: "These are my study notes on the Structure and Interpretation of Computer Programs (SICP)."
tags: [SICP, Computer Science]
keywords: SICP, Computer Science
toc: false

folder: /wiki/CompSci/SICP/
sidebar: compsci_sicp_sidebar
---

This is a difficult book to follow, but is arguably one of the best in the study of Computer Science. It is still used by MIT today.

This book is written such that it's assumed you have at least *some* previous exposure to programming. If you do not, it is highly recommended that you take an introductory course to a programming language such as Python.

Furthermore, this book also uses the Scheme dialect of the Lisp programming language, which is available for most common platforms. Scheme is a pedagogical language and contains only the essentials so that you can learn programming in more advanced languages later. It is highly recommended that you follow the original SICP book that uses Scheme rather than other iterations.

---

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
