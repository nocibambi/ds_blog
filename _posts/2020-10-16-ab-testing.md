---
keywords: fastai
title: A/B testing with Python
nb_path: _notebooks/2020-10-16-ab-testing.ipynb
layout: notebook
---

<!--
#################################################
### THIS FILE WAS AUTOGENERATED! DO NOT EDIT! ###
#################################################
# file to edit: _notebooks/2020-10-16-ab-testing.ipynb
-->

<div class="container" id="notebook-container">
        
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Introduction to A/B testing with python.</p>
<p>From the <a href="https://www.oreilly.com/library/view/data-science-from/9781492041122/">Data Science from Scratch book</a>.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Libraries-and-helper-functions">Libraries and helper functions<a class="anchor-link" href="#Libraries-and-helper-functions"> </a></h2>
</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">import</span> <span class="nn">math</span> <span class="k">as</span> <span class="nn">m</span>
<span class="kn">from</span> <span class="nn">typing</span> <span class="kn">import</span> <span class="n">Tuple</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">normal_probability_below</span><span class="p">(</span><span class="n">x</span><span class="p">:</span> <span class="nb">float</span><span class="p">,</span> <span class="n">mu</span><span class="p">:</span> <span class="nb">float</span> <span class="o">=</span> <span class="mi">0</span><span class="p">,</span> <span class="n">sigma</span><span class="p">:</span> <span class="nb">float</span> <span class="o">=</span> <span class="mi">1</span><span class="p">)</span> <span class="o">-&gt;</span> <span class="nb">float</span><span class="p">:</span>
    <span class="k">return</span> <span class="p">(</span><span class="mi">1</span> <span class="o">+</span> <span class="n">m</span><span class="o">.</span><span class="n">erf</span><span class="p">((</span><span class="n">x</span> <span class="o">-</span> <span class="n">mu</span><span class="p">)</span> <span class="o">/</span> <span class="n">m</span><span class="o">.</span><span class="n">sqrt</span><span class="p">(</span><span class="mi">2</span><span class="p">)</span> <span class="o">/</span> <span class="n">sigma</span><span class="p">))</span> <span class="o">/</span> <span class="mi">2</span>

<span class="k">def</span> <span class="nf">normal_probability_above</span><span class="p">(</span><span class="n">lo</span><span class="p">:</span> <span class="nb">float</span><span class="p">,</span> <span class="n">mu</span><span class="p">:</span> <span class="nb">float</span> <span class="o">=</span> <span class="mi">0</span><span class="p">,</span> <span class="n">sigma</span><span class="p">:</span> <span class="nb">float</span> <span class="o">=</span> <span class="mi">1</span><span class="p">)</span> <span class="o">-&gt;</span> <span class="nb">float</span><span class="p">:</span>
    <span class="k">return</span> <span class="mi">1</span> <span class="o">-</span> <span class="n">normal_probability_below</span><span class="p">(</span><span class="n">lo</span><span class="p">,</span> <span class="n">mu</span><span class="p">,</span> <span class="n">sigma</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">two_sided_p_value</span><span class="p">(</span><span class="n">x</span><span class="p">:</span> <span class="nb">float</span><span class="p">,</span> <span class="n">mu</span><span class="p">:</span> <span class="nb">float</span> <span class="o">=</span> <span class="mi">0</span><span class="p">,</span> <span class="n">sigma</span><span class="p">:</span> <span class="nb">float</span> <span class="o">=</span> <span class="mi">1</span><span class="p">)</span> <span class="o">-&gt;</span> <span class="nb">float</span><span class="p">:</span>
    <span class="sd">&quot;&quot;&quot;Return the probability of getting at least as extreme value as `x`, given</span>
<span class="sd">    that our values are from a normal distribution with `mu` mean and `sigma` std.</span>
<span class="sd">    &quot;&quot;&quot;</span>

    <span class="c1"># If x is greater than the mean return everything above x</span>
    <span class="k">if</span> <span class="n">x</span> <span class="o">&gt;=</span> <span class="n">mu</span><span class="p">:</span>
        <span class="k">return</span> <span class="mi">2</span> <span class="o">*</span> <span class="n">normal_probability_above</span><span class="p">(</span><span class="n">x</span><span class="p">,</span> <span class="n">mu</span><span class="p">,</span> <span class="n">sigma</span><span class="p">)</span>
    <span class="c1"># If x is less than the mean than return everything below x</span>
    <span class="k">else</span><span class="p">:</span>
        <span class="k">return</span> <span class="mi">2</span> <span class="o">*</span> <span class="n">normal_probability_below</span><span class="p">(</span><span class="n">x</span><span class="p">,</span> <span class="n">mu</span><span class="p">,</span> <span class="n">sigma</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="A/B-test">A/B test<a class="anchor-link" href="#A/B-test"> </a></h2>
</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">estimate_parameters</span><span class="p">(</span><span class="n">N</span><span class="p">:</span> <span class="nb">int</span><span class="p">,</span> <span class="n">true</span><span class="p">:</span> <span class="nb">int</span><span class="p">)</span> <span class="o">-&gt;</span> <span class="n">Tuple</span><span class="p">[</span><span class="nb">float</span><span class="p">,</span> <span class="nb">float</span><span class="p">]:</span>
    <span class="n">p</span> <span class="o">=</span> <span class="n">true</span> <span class="o">/</span> <span class="n">N</span>
    <span class="n">sigma</span> <span class="o">=</span> <span class="n">m</span><span class="o">.</span><span class="n">sqrt</span><span class="p">(</span><span class="n">p</span> <span class="o">*</span> <span class="p">(</span><span class="mi">1</span> <span class="o">-</span> <span class="n">p</span><span class="p">)</span> <span class="o">/</span> <span class="n">N</span><span class="p">)</span>
    <span class="k">return</span> <span class="n">p</span><span class="p">,</span> <span class="n">sigma</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>$H_0$: $p_a$ and $p_b$ are the same</p>
<p>With simplification this means that $p_a - p_b = 0$</p>

</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">a_b_test_statistic</span><span class="p">(</span><span class="n">N_A</span><span class="p">:</span> <span class="nb">int</span><span class="p">,</span> <span class="n">A</span><span class="p">:</span> <span class="nb">int</span><span class="p">,</span> <span class="n">N_B</span><span class="p">:</span> <span class="nb">int</span><span class="p">,</span> <span class="n">B</span><span class="p">:</span><span class="nb">int</span><span class="p">)</span> <span class="o">-&gt;</span> <span class="nb">float</span><span class="p">:</span>
    <span class="n">p_A</span><span class="p">,</span> <span class="n">sigma_A</span> <span class="o">=</span> <span class="n">estimate_parameters</span><span class="p">(</span><span class="n">N_A</span><span class="p">,</span> <span class="n">A</span><span class="p">)</span>
    <span class="n">p_B</span><span class="p">,</span> <span class="n">simga_B</span> <span class="o">=</span> <span class="n">estimate_parameters</span><span class="p">(</span><span class="n">N_B</span><span class="p">,</span> <span class="n">B</span><span class="p">)</span>

    <span class="k">return</span> <span class="p">(</span><span class="n">p_B</span> <span class="o">-</span> <span class="n">p_A</span><span class="p">)</span> <span class="o">/</span> <span class="n">m</span><span class="o">.</span><span class="n">sqrt</span><span class="p">(</span><span class="n">sigma_A</span> <span class="o">**</span> <span class="mi">2</span> <span class="o">+</span> <span class="n">simga_B</span> <span class="o">**</span> <span class="mi">2</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Example-1">Example 1<a class="anchor-link" href="#Example-1"> </a></h2>
</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">z</span> <span class="o">=</span> <span class="n">a_b_test_statistic</span><span class="p">(</span><span class="mi">1000</span><span class="p">,</span> <span class="mi">200</span><span class="p">,</span> <span class="mi">1000</span><span class="p">,</span> <span class="mi">180</span><span class="p">)</span>
<span class="n">z</span><span class="p">,</span> <span class="n">two_sided_p_value</span><span class="p">(</span><span class="n">z</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">



<div class="output_text output_subarea output_execute_result">
<pre>(-1.1403464899034472, 0.2541419765422359)</pre>
</div>

</div>

</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>That is, the probability of at least such a big difference occurring assuming that the two probabilities are the same is ~0.25.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Example-2">Example 2<a class="anchor-link" href="#Example-2"> </a></h2>
</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Let's decrease the occurance of $b$ even more.</p>

</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">z</span> <span class="o">=</span> <span class="n">a_b_test_statistic</span><span class="p">(</span><span class="mi">1000</span><span class="p">,</span> <span class="mi">200</span><span class="p">,</span> <span class="mi">1000</span><span class="p">,</span> <span class="mi">150</span><span class="p">)</span>
<span class="n">z</span><span class="p">,</span> <span class="n">two_sided_p_value</span><span class="p">(</span><span class="n">z</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">



<div class="output_text output_subarea output_execute_result">
<pre>(-2.948839123097944, 0.003189699706216853)</pre>
</div>

</div>

</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>That is the probability of at least this large difference to occur if the probabilities are the same is 0.03.</p>

</div>
</div>
</div>
</div>
 

