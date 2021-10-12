---
author: Alessandro Cubeddu
title: Power absorbtion forecasting
subtitle: Comparison between some methods
date: 2021-04-01
theme: beige # https://revealjs.com/themes/
transition: fade # https://revealjs.com/transitions/
# see all the options here: https://revealjs.com/config/
highlight-style: breezeDark
progress: true
slideNumber: true
hash: true
navigationMode: linear
margin: -0.1
width: 1700



autoPlayMedia: true
---
<div class="im_back">
<div>
<ul style="color:blue" class="problem_size">Facts:
<li>A land like Italy absorbes on average 33GW 
    of electrical energy each minute</li>
<li>The energy demand has huge fluctuaction around the mean ...</li>
</ul>
</div>

<div class="space-between">
<br />
<span style="color:red">Question: How can I predict the power absorption?</span>  
<span style="color:green">Regression methods or Deep Learning</span> 
</div>    
    
<div class="space-between">    
<span style="color:red">Question: Which method is better?</span>   
<span style="color:green">Not easy answer ...</span>  
</div>
<p style="font-weight: bold;">
Let's compare some models to find out the answer  
</p>
</div>

### Where to start?

###

<div class="margin_minus">
<p class="model_names">Power absorbtion hour by hour</p>
<img src="images/power.svg" 
         alt="!D Power absorption full"
         width="8000" 
         height="400" />
<ul>
<li>Information not well human readable ... too many values</li>
<li>Target of models: discover seasonalities and deliver good prediction</li>
<li>Training: 30000 inputs, Test: 15000 inputs</li>
</ul>
<!--/div>
<div margin-bottom="-1cm">   
<p class="model_names">Power absorbtion weekly averaged</p>
<img src="images/power-weekly.svg" 
         alt="!D Power absorption weekly"
         width="7000" 
         height="300" /-->
</div>              


### Chosen models

###

|<span style="font-weight:bold">Model</span> | <span style="font-weight:bold">Kind</span> | <span style="font-weight:bold"> Has memory?</span> | <span style="font-weight:bold">How learns</span> |
| :---:| :---: | :---: | :---: |
| <span style="color:green">Ridge</span> | shallow | NO | whole train (once 30000 inputs) 
| <span style="color:purple">FB Prophet</span> | shallow  | NO | whole train (once 30000 inputs)| 
| <span style="color:blue">LSTM</span> | deep  | YES | subsets (many 120 inputs)| 
| <span style="color:orange">GRU</span>| deep | YES | subsets (many 120 inputs)| 
| <span style="color:red">CNN 1D</span>| deep | NO but HAS FILTERS | subset (many 120 inputs)|


### How do the models do...

###
</br>
<div class="table_font">
<div>
|<span style="font-weight:bold">Model</span> | <span style="font-weight:bold">Features</span> | <span style="font-weight:bold">Inputs Dim</span>| <span style="font-weight:bold"> Output</span> | 
|:---:|:---:|:---:|:---:|
| <span style="color:green">Ridge</span>|7<br /> hour; day; <br /> week;  day_of_week; <br /> month;  year; holidays |(1x7)| 1<br />Power at the <br /> selected date|
| <span style="color:purple">FB Prophet</span>|1<br /> Timestamp | (1x1)| 1<br /> Power at the <br /> selected date|
| <span style="color:blue">LSTM</span><br /><span style="color:orange">GRU</span><br /><span style="color:red">CNN 1D</span>|2 <br /> Power; holidays |(120x2)| 1<br />Power 24 hours <br /> in the future|
</div>
</div>

### Architecture of the models

###
<div class="margin_minus">
<div class="center_align">
<img 
     src="images/architectures.svg" 
     alt="!D fig1"
     width="1800" 
     height="800" /></div>
</div>


<!--div class="architecture">

<div class="columns_2"> 
<div class="center_align">
<p class="medium_size">Ridge</p></div>
<div class="center_align">
<img 
    src="graph/logReg_graph.svg"
    alt="!D lstm neural network"
    height="60"
    width="100" />
</div> </div>
    
<div class="columns_2"> 
<div class="center_align">
<p class="medium_size">fb Prophet</p></div>
<div class="center_align">
<img
    src="graph/fbProphet.svg"
    alt="!D lstm neural network"
    height="100"
     width="300" />
</div> </div>    
    
    
<div class="columns_2"> 
<div class="center_align">
<p class="medium_size">Long-Short Term Memory</p></div>
<div class="center_align">
<img 
    src="graph/lstm_graph.svg"
    alt="!D lstm neural network"
    height="100"
     width="300" />
</div> </div>

<div class="columns_2"> 
<div class="center_align">
<p class="medium_size">1D convolutional NN</p> </div>  
<div class="center_align">
<img 
    src="graph/CNN_graph.svg"
    alt="!D convolutional neural network"
    height="100"
    width="500" />
</div></div>
    
<div class="columns_2">  
<div class="center_align">
<p class="medium_size">Gated Recurrent Unit</p></div>
<div class="center_align">
<img 
    src="graph/GRU_graph.svg"
    alt="!D GRU neural network"
    height="100"
    width="300" />
</div></div>
</div-->



### How do the 5 models perform ...

###
<!--p class="results">
Input: 120 hours in the past (5 day)  
Output: Power absorbtion 24 hours in the future</p-->

<!--![](images/fig_2018-06-01.png){ width=27%}-->
<div class="columns_new"> 
<div class="right_align">
<img 
     src="images/fig_2019-03-19.png" 
     alt="!D fig1"
     width="650" 
     height="400" /></div>
<div class="left_align">
<img 
     src="images/fig_2020-01-05.png" 
     alt="!D fig2"
     width="650" 
     height="400" /></div>
<!--![](images/fig_2020-01-05.png){ width=30%}-->
<br/>
<div class="spanning">
<ul>
<li>The input for LSTM, CNN and GRU is the history curve </li>
<li>The input for Ridge and fb Prophet is the target date </li>
<li>Output: Power absorption at target date</li>
</ul>
</div>
</div>

### What's the best model?

###
<div class="columns_2">
<div class="right_aling" style="margin: -10%">    
<img 
    src="images/smape_last.png"
    alt="!D Error and computation time"
    height="700"
    width="600" />
</div>
<br/> 
<div class="left_align">
::: incremental
- CNN 1D is the most performant
<p class="space-between"> </p>
- Ridge is trained in less than  
  one minute
<p class="space-between"> </p>
<p class="space-between"> </p>
:::

<br  /> 
    
<div class="red">    
<p class="space-between"> </p>    
::: incremental
* If you need "high" precision ->  
    go for <a style="font-weight: bold;">CNN 1D</a>
<p class="space-between"> </p>
* If you need a "rapid" answer ->  
  go for <a style="font-weight: bold;">Ridge</a>
:::  
</div>
    
</div>
    
</div>

### 
<div class="columns_2">
<div class="icons">
<img 
    src="icons/many_linux.svg"
    alt="!D GRU neural network"
    height="800"
    width="800" />
</div>
<br />
<h2 color="blue">Thanks!</h2>
<br />
<ul font-weight="bold"> Many thanks to: </ul>

<ul>
<br />
<li> Agentur für Arbeit - Düsseldorf</li>
<li> Spiced Academy </li>
<li> Malte and Sam </li>
<li> My mates: JJJLLM</li>
<li> Kathrin </li>
<li>[Net2Vis - Ulm University](https://viscom.net2vis.uni-ulm.de/)</li>
</ul>
</div>
<div>
<!--ref class="smallfont" url("https://viscom.net2vis.uni-ulm.de/")>Net2Vis for architecture representations</ref-->

</div>


