---
keywords: fastai
title: Stochastic gradient descent
nb_path: _notebooks/2020-10-22-stochastic-gradient-descent.ipynb
layout: notebook
---

<!--
#################################################
### THIS FILE WAS AUTOGENERATED! DO NOT EDIT! ###
#################################################
# file to edit: _notebooks/2020-10-22-stochastic-gradient-descent.ipynb
-->

<div class="container" id="notebook-container">
        
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
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
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">from</span> <span class="nn">typing</span> <span class="kn">import</span> <span class="n">List</span>
<span class="kn">import</span> <span class="nn">random</span>
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
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">Vector</span> <span class="o">=</span> <span class="n">List</span><span class="p">[</span><span class="nb">float</span><span class="p">]</span>
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
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">add</span><span class="p">(</span><span class="n">vector1</span><span class="p">:</span> <span class="n">Vector</span><span class="p">,</span> <span class="n">vector2</span><span class="p">:</span> <span class="n">Vector</span><span class="p">)</span> <span class="o">-&gt;</span> <span class="n">Vector</span><span class="p">:</span>
    <span class="k">assert</span> <span class="nb">len</span><span class="p">(</span><span class="n">vector1</span><span class="p">)</span> <span class="o">==</span> <span class="nb">len</span><span class="p">(</span><span class="n">vector2</span><span class="p">)</span>
    <span class="k">return</span> <span class="p">[</span><span class="n">v1</span> <span class="o">+</span> <span class="n">v2</span> <span class="k">for</span> <span class="n">v1</span><span class="p">,</span> <span class="n">v2</span> <span class="ow">in</span> <span class="nb">zip</span><span class="p">(</span><span class="n">vector1</span><span class="p">,</span> <span class="n">vector2</span><span class="p">)]</span>


<span class="k">def</span> <span class="nf">scalar_multiply</span><span class="p">(</span><span class="n">c</span><span class="p">:</span> <span class="nb">float</span><span class="p">,</span> <span class="n">vector</span><span class="p">:</span> <span class="n">Vector</span><span class="p">)</span> <span class="o">-&gt;</span> <span class="n">Vector</span><span class="p">:</span>
    <span class="k">return</span> <span class="p">[</span><span class="n">c</span> <span class="o">*</span> <span class="n">v</span> <span class="k">for</span> <span class="n">v</span> <span class="ow">in</span> <span class="n">vector</span><span class="p">]</span>


<span class="k">def</span> <span class="nf">gradient_step</span><span class="p">(</span><span class="n">v</span><span class="p">:</span> <span class="n">Vector</span><span class="p">,</span> <span class="n">gradient</span><span class="p">:</span> <span class="n">Vector</span><span class="p">,</span> <span class="n">step_size</span><span class="p">:</span> <span class="nb">float</span><span class="p">)</span> <span class="o">-&gt;</span> <span class="n">Vector</span><span class="p">:</span>
    <span class="sd">&quot;&quot;&quot;Return vector adjusted with step. Step is gradient times step size.</span>
<span class="sd">    &quot;&quot;&quot;</span>
    <span class="n">step</span> <span class="o">=</span> <span class="n">scalar_multiply</span><span class="p">(</span><span class="n">step_size</span><span class="p">,</span> <span class="n">gradient</span><span class="p">)</span>
    <span class="k">return</span> <span class="n">add</span><span class="p">(</span><span class="n">v</span><span class="p">,</span> <span class="n">step</span><span class="p">)</span>

<span class="k">def</span> <span class="nf">linear_gradient</span><span class="p">(</span><span class="n">x</span><span class="p">:</span> <span class="nb">float</span><span class="p">,</span> <span class="n">y</span><span class="p">:</span> <span class="nb">float</span><span class="p">,</span> <span class="n">theta</span><span class="p">:</span> <span class="n">Vector</span><span class="p">)</span> <span class="o">-&gt;</span> <span class="n">Vector</span><span class="p">:</span>
    <span class="n">slope</span><span class="p">,</span> <span class="n">intercept</span> <span class="o">=</span> <span class="n">theta</span>
    <span class="n">predicted</span> <span class="o">=</span> <span class="n">slope</span> <span class="o">*</span> <span class="n">x</span> <span class="o">+</span> <span class="n">intercept</span>
    <span class="n">error</span> <span class="o">=</span> <span class="p">(</span><span class="n">predicted</span> <span class="o">-</span> <span class="n">y</span><span class="p">)</span> <span class="c1">#** 2</span>
    <span class="c1"># print(x, y, theta, predicted, error)</span>
    <span class="k">return</span> <span class="p">[</span><span class="mi">2</span> <span class="o">*</span> <span class="n">error</span> <span class="o">*</span> <span class="n">x</span><span class="p">,</span> <span class="mi">2</span> <span class="o">*</span> <span class="n">error</span><span class="p">]</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Stochastic-gradients">Stochastic gradients<a class="anchor-link" href="#Stochastic-gradients"> </a></h2>
</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Here we use one training example at a time to calculate the gradient steps</p>

</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">inputs</span> <span class="o">=</span> <span class="p">[(</span><span class="n">x</span><span class="p">,</span> <span class="mi">20</span> <span class="o">*</span> <span class="n">x</span> <span class="o">+</span> <span class="mi">5</span><span class="p">)</span> <span class="k">for</span> <span class="n">x</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="o">-</span><span class="mi">50</span><span class="p">,</span> <span class="mi">50</span><span class="p">)]</span>

<span class="n">theta</span> <span class="o">=</span> <span class="p">[</span><span class="n">random</span><span class="o">.</span><span class="n">uniform</span><span class="p">(</span><span class="o">-</span><span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">),</span> <span class="n">random</span><span class="o">.</span><span class="n">uniform</span><span class="p">(</span><span class="o">-</span><span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">)]</span>
<span class="n">learning_rate</span> <span class="o">=</span> <span class="mf">0.001</span>


<span class="k">for</span> <span class="n">epoch</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="mi">100</span><span class="p">):</span>
    <span class="k">for</span> <span class="n">x</span><span class="p">,</span> <span class="n">y</span> <span class="ow">in</span> <span class="n">inputs</span><span class="p">:</span>
        <span class="n">grad</span> <span class="o">=</span> <span class="n">linear_gradient</span><span class="p">(</span><span class="n">x</span><span class="p">,</span> <span class="n">y</span><span class="p">,</span> <span class="n">theta</span><span class="p">)</span>
        <span class="n">theta</span> <span class="o">=</span> <span class="n">gradient_step</span><span class="p">(</span><span class="n">theta</span><span class="p">,</span> <span class="n">grad</span><span class="p">,</span> <span class="o">-</span><span class="n">learning_rate</span><span class="p">)</span>
    <span class="nb">print</span><span class="p">(</span><span class="n">epoch</span><span class="p">,</span> <span class="n">theta</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">
<pre>0 [20.108274621088928, -0.3890550572184463]
1 [20.103628550173042, -0.15784430337372637]
2 [20.09918250047512, 0.06344662581205483]
3 [20.094927182760102, 0.2752433412342787]
4 [20.090854449810823, 0.47795318030884215]
5 [20.086956448727392, 0.6719660044610173]
6 [20.0832257045743, 0.8576549486610185]
7 [20.07965500742264, 1.0353771386943718]
8 [20.076237509713653, 1.2054743780282082]
9 [20.07296662801438, 1.3682738056445862]
10 [20.06983608174483, 1.5240885250583422]
11 [20.06683986242782, 1.6732182068542554]
12 [20.063972181799524, 1.8159496643823287]
13 [20.06122752813499, 1.9525574051756995]
14 [20.05860064833006, 2.083304159961789]
15 [20.056086446554076, 2.2084413869124764]
16 [20.05368014168373, 2.328209755991209]
17 [20.051377044125662, 2.442839611270341]
18 [20.049172772009843, 2.552551414011119]
19 [20.047063092087537, 2.6575561678668613]
20 [20.045043898683737, 2.758055822780714]
21 [20.04311134626141, 2.8542436641396707]
22 [20.04126169735246, 2.9463046849793786]
23 [20.039491436402585, 3.0344159418586236]
24 [20.037797100119768, 3.118746894557255]
25 [20.0361754521855, 3.19945973173537]
26 [20.034623390938133, 3.276709684335131]
27 [20.03313793652982, 3.3506453237233305]
28 [20.03171617359179, 3.421408845892839]
29 [20.030355435914238, 3.4891363463999814]
30 [20.02905307411413, 3.5539580824049044]
31 [20.02780658595349, 3.6159987219880825]
32 [20.026613585687915, 3.6753775847836647]
33 [20.025471758004617, 3.732208870958282]
34 [20.024378926653775, 3.7866018810682345]
35 [20.023332973880006, 3.8386612263190933]
36 [20.022331891127603, 3.8884870294569893]
37 [20.021373782454166, 3.936175118240505]
38 [20.020456760427248, 3.981817208697045]
39 [20.019579095445714, 4.0255010817522585]
40 [20.018739087243826, 4.067310752589061]
41 [20.017935091021968, 4.107326630985043]
42 [20.017165620461952, 4.14562567751338]
43 [20.016429140217177, 4.182281550851487]
44 [20.015724268710606, 4.217364749099938]
45 [20.015049636287394, 4.250942746103223]
46 [20.014403952714495, 4.283080120704653]
47 [20.01378597502875, 4.313838681185705]
48 [20.013194514460565, 4.343277583993619]
49 [20.012628414045615, 4.371453447141006]
50 [20.01208658825722, 4.398420459215999]
51 [20.011568035372246, 4.424230484791519]
52 [20.011071730850883, 4.448933163459061]
53 [20.010596714895236, 4.472576004466116]
54 [20.01014208044218, 4.495204478772731]
55 [20.009706939528638, 4.5168621063046]
56 [20.009290475161325, 4.5375905399679235]
57 [20.008891875158, 4.557429645750311]
58 [20.008510370231065, 4.576417578971538]
59 [20.00814524887334, 4.594590858346274]
60 [20.007795789585977, 4.611984435823321]
61 [20.00746133605139, 4.628631763741193]
62 [20.00714119205082, 4.644564858428533]
63 [20.00683481657478, 4.659814363112007]
64 [20.00654157694565, 4.674409606833622]
65 [20.00626093700885, 4.688378660053233]
66 [20.0059923009138, 4.701748388289354]
67 [20.005735205576336, 4.714544504461379]
68 [20.00548916033653, 4.726791619391082]
69 [20.00525365468124, 4.738513287317169]
70 [20.005028243503418, 4.749732051371429]
71 [20.004812513452155, 4.760469488056405]
72 [20.0046060409254, 4.770746248364355]
73 [20.004408419316054, 4.780582096925131]
74 [20.004219291985194, 4.789995950689194]
75 [20.004038256610244, 4.799005914654827]
76 [20.00386500754045, 4.807629317187821]
77 [20.003699183947933, 4.815882743439104]
78 [20.00354046799362, 4.823782066495483]
79 [20.003388552379008, 4.831342478409528]
80 [20.003243202425267, 4.838578520565049]
81 [20.003104049179598, 4.8455041097305935]
82 [20.002970873552083, 4.852132564899691]
83 [20.0028434121415, 4.858476634395222]
84 [20.00272141143189, 4.864548519281559]
85 [20.002604655271004, 4.870359897369896]
86 [20.002492920554257, 4.875921945826025]
87 [20.00238595286676, 4.881245361497983]
88 [20.002283589604943, 4.886340382432449]
89 [20.0021855928701, 4.8912168074015]
90 [20.002091842376988, 4.8958840153869385]
91 [20.002002090950178, 4.90035098289899]
92 [20.001916203724086, 4.904626300833264]
93 [20.001833987421463, 4.908718191643037]
94 [20.00175530458321, 4.912634524898607]
95 [20.001680007220763, 4.916382832992619]
96 [20.001607906672934, 4.919970324321837]
97 [20.001538916328176, 4.923403898248704]
98 [20.001472898762803, 4.926690159008146]
99 [20.001409710601003, 4.929835427066589]
</pre>
</div>
</div>

</div>
</div>

</div>
    {% endraw %}

</div>
 

