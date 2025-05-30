---
keywords: fastai
title: InfluxDB organizing concepts (Organization, Buckets, Data elements)
nb_path: _notebooks/2020-10-30-influxdb-concepts-data-bucket.ipynb
layout: notebook
---

<!--
#################################################
### THIS FILE WAS AUTOGENERATED! DO NOT EDIT! ###
#################################################
# file to edit: _notebooks/2020-10-30-influxdb-concepts-data-bucket.ipynb
-->

<div class="container" id="notebook-container">
        
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Organizations">Organizations<a class="anchor-link" href="#Organizations"> </a></h2><p>Organizations behave as workspaces. They can possess</p>
<ul>
<li>a user or a group of users,</li>
<li>dashboards</li>
<li>tasks</li>
<li>buckets</li>
</ul>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Buckets">Buckets<a class="anchor-link" href="#Buckets"> </a></h2><p>InfluxDB data is stored in buckets. Each bucket belongs to an organization.</p>
<p>Buckets are basically databases with the important addition of retention policies.</p>
<p>A bucket's <strong>retention policy</strong> determines the time duration for which its datapoints persist.</p>
<h3 id="Bucket-creation">Bucket creation<a class="anchor-link" href="#Bucket-creation"> </a></h3><p>You can create bucket on the UI either in the</p>
<ul>
<li><strong>Data Explorer</strong>, or</li>
<li>in the <strong>Load Data</strong> menu points.</li>
</ul>
<p>Here you need to give a name and also set when would you like to delete the data.</p>
<h3 id="Edit-buckets">Edit buckets<a class="anchor-link" href="#Edit-buckets"> </a></h3><p>You can view your existing buckets and edit them under the <strong>Data</strong> / <strong>Load Data</strong> menu points.</p>
<h3 id="Delete-buckets">Delete buckets<a class="anchor-link" href="#Delete-buckets"> </a></h3><p>You can delet buckets under the <strong>Data</strong> / <strong>Load Data</strong> menu points.</p>
<h3 id="Explore-bucket-data">Explore bucket data<a class="anchor-link" href="#Explore-bucket-data"> </a></h3><p>In the <strong>Data</strong> / <strong>Load Data</strong> menu point, you can simply click on a bucket to open it in the <strong>Data Explorer</strong></p>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Data-elements">Data elements<a class="anchor-link" href="#Data-elements"> </a></h2>
</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<ul>
<li><code>_time</code>, timestamp (in epoch nanosecond)</li>
<li><code>_measurement</code>, string</li>
<li><code>_field</code>, string: field key</li>
<li><code>_value</code>, string, floats, integers, or booleans: field value</li>
</ul>
<p>Fields are not indexed, and required. Tags are indexed but are not required.</p>
<p>An additional element is a 'field set' which are the collection of field key-value pairs having the same timestamp.</p>
<p>Tags: these are simple column-like structure where the column name is the 'tag key' and its content are 'tag values'.</p>
<p>Tag sets are the tag-key value combinations in the data.</p>
<p>As tags are indexed, they are more performant than fields. For this reason, often, you might want to restructure your tables so your fields will become tags and the other way around.</p>
<p>With tags having many unique values, however, look out for <a href="https://docs.influxdata.com/influxdb/v2.0/reference/glossary/#series-cardinality">series cardinality</a> as it can eat up your memory!</p>
<h3 id="Series">Series<a class="anchor-link" href="#Series"> </a></h3><p>Series key is a collection of points which have the same</p>
<ul>
<li>measurement,</li>
<li>tag set, and</li>
<li>field key.</li>
</ul>
<p>Their use is a core element in definint the data schema.</p>
<h3 id="Points">Points<a class="anchor-link" href="#Points"> </a></h3><p>A <strong>Point</strong> consists of a series key, a field value, and a timestamp.</p>

</div>
</div>
</div>
</div>
 

