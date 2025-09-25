---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
# background: /CL_iconSequence-removebgbis.png
background: /photo1
#https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Cosmolattice on GPUs
info: |
  ## Cosmolattice on GPUs

colorSchema: light

# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# open graph
# seoMeta:
#  ogImage: https://cover.sli.dev
addons:
  - "slidev-addon-stem"
  - fancy-arrow
---


<img src="/CL_iconSequence.png" style="width:560px;height:auto; bottom:0; right:0; position:fixed">
<div style="margin-top:2cm;">

# Parallelization in CosmoLattice

September 2025, Daejeon

Franz R. Sattler

<small>
Bielefeld University
</small>

&nbsp;

</div>

---
layout: default
transition: slide-up
---

<div class="grid grid-cols-[48%_56%] gap-3">
<div>

# Why can we parallelize?

And why would we want to?

Example: Solving massless Klein-Gordon equation, $d=1$

<v-switch>
<template #0>
$$
\begin{aligned}
\partial_t^2 \phi(i) = \Delta^+_i\Delta^-_i \phi(i)
\end{aligned}
$$
</template>
<template #1-7>
$$
\begin{aligned}
\partial_t \pi(i) &= \Delta^+_i\Delta^-_i \phi(i) \,, \\
\partial_t \phi(i) &= \pi(i) \,.
\end{aligned}
$$

*(Leapfrog scheme.)*
</template>
</v-switch>

<v-click at=2>

Stencil of update for **one** lattice site is $s = 1$
</v-click>

<v-click at=3>
&nbsp;&nbsp;  <mdi-arrow-right/> Only local information needed (neighbours).
</v-click>

</div>

<div style="margin-top:30mm">
<ImageFigure
  src="/1DGridStencil.png"
  caption=""
  width="100%"
  height="auto"
/>

<v-click at=4>

## We could compute all lattice sites independently!

*See lecture on Friday!*

</v-click>

</div>

</div>

<div class="grid grid-cols-[15%_80%] gap-3">
<div></div>
<div style="margin-top:8mm">
<v-click at=6>
Less granular: split <h2>sub-regions</h2> of lattice across many computers <h2>(nodes)</h2>
</v-click>
</div>
</div>

<style>
.katex {
    font-size: 1.0em;
}
h2 {
    background-color: #2B90B6;
    background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
  display: inline;
}
</style>


---
layout: fact
transition: slide-down
---

<h1> Parallelization </h1> of CosmoLattice simulations requires to split both <h2>computation</h2> and <h2>data</h2> across <h2>cores</h2>.

<div v-click="[1,3]" style="margin-top:-5mm;">
<br><br>
<br>
<h2>Cores:</h2> <b>Nodes</b> (distributed) and <b> Threads </b> (shared).
</div>

<div v-click="[2,3]">
<div class="grid grid-cols-[15%_60%] gap-3" style="margin-top:10mm;">
<div></div>
<div>

| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<h3>Type</h3> | <h2>distributed</h2> | <h2>shared</h2>  |
| --- | -- | ---- |
| <h3>Data</h3> | split between <b>nodes</b> | shared by all <b>threads</b> |
| <h3>Computation</h3> | split between <b>nodes</b> | split between <b>threads</b> |
</div>
</div>
</div>

<div v-click=3 style="position:absolute; top:250px; left:50px;">
<div class="grid grid-cols-[35%_35%_70%]">

<div v-click=4>
<ImageFigure
  src="/amdahl.png"
  caption=""
  width="95%"
  height="auto"
/>
</div>

<div>
$$
\textrm{speedup} = \frac{1}{(1-\alpha)+\frac{\alpha}{n_\textrm{cores}}}
$$
<br>

CosmoLattice: $\alpha \gtrsim 0.99$ <br><small>(see manual)</small>

</div>

<div style="margin-left:-55mm">
<br>
<br>

$\alpha$ = part of the code which runs in parallel
<br>
$n_\textrm{cores}$ = speedup of the parallel part
</div>

</div>
</div>

<style>
h1 {
  display: inline;
}
h3 {
  display: inline;
}
h2 {
    background-color: #2B90B6;
    background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
  display: inline;
}
</style>

---
layout: default
transition: fade
---

<div class="grid grid-cols-[48%_56%] gap-3">
<div>

# Why can we parallelize?

And why would we want to?

Example: Solving massless Klein-Gordon equation, $d=1$

$$
\begin{aligned}
\partial_t \pi(i) &= \Delta^+_i\Delta^-_i \phi(i) \,, \\
\partial_t \phi(i) &= \pi(i) \,.
\end{aligned}
$$

*(Leapfrog scheme.)*

Stencil of update for **one** lattice site is $s = 1$

&nbsp;&nbsp;  <mdi-arrow-right/> Only local information needed (neighbours).
<br><br>&nbsp;&nbsp;  <mdi-arrow-right/> Each site can be updated independently.
</div>

<div style="margin-top:23mm">
<v-switch>
<template #0>
<div style="margin-top:7mm;margin-left:1mm">
<ImageFigure
  src="/1DGridStencil.png"
  caption=""
  width="87%"
  height="auto"
/>
</div>
</template>
<template #1-3>
<ImageFigure
  src="/1DGridStencil_nodes.png"
  caption=""
  width="88%"
  height="auto"
/>
<v-click at=2>
<h2>Problem:</h2> Data is missing on node 1
</v-click>
</template>
<template #3-5>
<div style="margin-top:1mm">
<ImageFigure
  src="/1DGridStencil_ghosts.png"
  caption=""
  width="100%"
  height="auto"
/>
</div>
<h2>Solution:</h2> Use <h3>ghosts</h3>.

Ghosts are local copies of data on other nodes.

</template>

</v-switch>

</div>
</div>

<v-click at=4>
<div class="grid grid-cols-[14%_80%] gap-3" style="margin-top:10mm">
<div></div>
<h2>Need to update ghosts after every time-step.</h2>
</div>
</v-click>


<style>
.katex {
    font-size: 1.0em;
}
h3 {
  display: inline;
}
h2 {
    background-color: #2B90B6;
    background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
  display: inline;
}
</style>

---
layout: default
transition: slide-left
---

<div class="grid grid-cols-[48%_56%] gap-3">
<div>

# Data communication

The standard for communication in distributed-memory applications:

<b>M</b>essage <b>P</b>assing <b>I</b>nterface (MPI)

Exchange ghost data between **nodes** over the **network** automatically if anything changes.

</div>

<div style="margin-top:24mm">
<ImageFigure
  src="/1DGridStencil_ghosts.png"
  caption=""
  width="100%"
  height="auto"
/>
</div>
</div>

<div class="grid grid-cols-[24%_80%] gap-3" style="margin-top:2mm">
<div></div>
<v-click>
<i>CosmoLattice does this automatically under the hood!</i>
</v-click>
</div>

<v-click>
<div class="grid grid-cols-[34%_60%] gap-3" style="margin-top:15mm">

## How to turn it on?

```bash
$ cd build/
$ cmake -DMPI=ON ..
$ make
$ mpirun -n 16 ./lphi4 input=./lphi4.in
```
</div>
<div class="grid grid-cols-[34%_60%] gap-3" style="margin-top:5mm" v-click>

<div>You may need to install fftw3 with MPI support.</div>

<div>
```bash
$ sudo apt-get install libfftw3-mpi-dev 
```
</div>

</div>
</v-click>


---
layout: default
transition: slide-left
---

<div class="grid grid-cols-[70%_30%] gap-3">
<div>

# FFT parallelization


-  **FFTW** supports parallelization along 1 direction.

<v-clicks at=1>

-  Improved scaling for many nodes: **PFFT** allows for parallelization in any dimensions.

-  To keep data transfer due to ghost exchange manageable, parallelization along $d-1$ directions.
</v-clicks>

<v-switch at=3>
<template #1>
<div class="grid grid-cols-[33%_33%_33%] gap-3" style="margin-bottom:-15mm; margin-top:-2mm">
<div>
<h1 style="text-align:center;">1D</h1>
$$
N = n_p * m
$$
</div>
<div style="margin-top:11mm">
$$
\begin{align*}
  N = 50
\end{align*}
$$
</div>
<div>
<h1 style="text-align:center;">2D</h1>
$$
\begin{align*}
N &= n^{(1)}_p * m \\
  &= n^{(2)}_p * m
\end{align*}
$$
</div>
</div>
</template>

<template #2>
<div class="grid grid-cols-[33%_33%_33%] gap-3" style="margin-bottom:-15mm; margin-top:-2mm">
<div>
<h1 style="text-align:center;">1D</h1>
$$
N = n_p * m
$$
</div>
<div style="margin-top:11mm">
$$
\begin{align*}
  N = 50
\end{align*}
$$
</div>
<div>
<h1 style="text-align:center;">2D</h1>
$$
\begin{align*}
N &= n^{(1)}_p * m \\
  &= n^{(2)}_p * m
\end{align*}
$$
</div>
</div>
<div > &nbsp;</div>
<div class="grid grid-cols-[50%_50%] gap-3" style="margin-top:2mm">
<div>
$$
\begin{align*}
n_p \in \{1,2,5,10,25,50\}
\end{align*}
$$
</div>
<div>
$$
\begin{align*}
(n_p^{(1)}, n_p^{(2)}) \in \{
  &(2, 1),(2,2), \\
  &(5, 1),(5,2), \\
  &(10, 2),(5,5), \\
  &(10, 5),(10,10), \\
  &(25, 5),(25,10), \\
  &(25, 25),(25,10)
\}
\end{align*}
$$
</div>
</div>
</template>

<template #3>
<div class="grid grid-cols-[33%_33%_33%] gap-3" style="margin-bottom:-15mm; margin-top:-2mm">
<div>
<h1 style="text-align:center;">1D</h1>
$$
N = n_p * m
$$
</div>
<div style="margin-top:11mm">
$$
\begin{align*}
  N = 50
\end{align*}
$$

<br>
Maximum parallelization
</div>
<div>
<h1 style="text-align:center;">2D</h1>
$$
\begin{align*}
N &= n^{(1)}_p * m \\
  &= n^{(2)}_p * m
\end{align*}
$$
</div>
</div>
<div > &nbsp;</div>
<div class="grid grid-cols-[50%_50%] gap-3">
<div  style="margin-left:16mm;">
<br>

$50$ nodes.
</div>
<div  style="margin-left:35mm;">
<br>

$25^2 = 625$ nodes.
</div>
</div>
</template>

<template #4>
<div class="grid grid-cols-[10%_75%] gap-3">
<div></div>
<div>
<ImageFigure
  src="/speed_up.png"
  caption="Figure 3: Speed up factor in parallelized simulations as a number of cores (tested on the Gacrux cluster
from the EPFL HPC center SCITAS, Switzerland). 
"
  width="82%"
  height="auto"
/>

<div style="font-size:0.5em;margin-top:-2mm"><i>Figure from arXiv:2102.01031 [astro-ph.CO]</i></div>
<br>
</div>
</div>
</template #0>
</v-switch>
</div>

<div style="margin-top:-5mm">
<ImageFigure
  src="/Cube_1D.png"
  caption=""
  width="100%"
  height="auto"
/>

<v-click at=1>
<ImageFigure
  src="/Cube_2D.png"
  caption=""
  width="100%"
  height="auto"
/>
</v-click>
</div>
</div>

<style>
.katex {
    font-size: 1.0em;
}
</style>

---
layout: end
transition: slide-up
---

# Questions?

Tomorrow: Shared-memory parallelization with GPUs.


---
layout: cover
background: /photo2
---

<img src="/CL_iconSequence.png" style="width:560px;height:auto; bottom:0; right:0; position:fixed">

<div style="margin-top:2cm;">

# CosmoLattice on GPUs

September 2025, Daejeon

Franz R. Sattler

<small>
Bielefeld University
</small>

&nbsp;


<a href="https://github.com/satfra/DiFfRG-introduction">
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 -3600 15872 16384">
  <path d="M165.9 397.4c0 2-2.3 3.6-5.2 3.6-3.3 .3-5.6-1.3-5.6-3.6 0-2 2.3-3.6 5.2-3.6 3-.3 5.6 1.3 5.6 3.6zm-31.1-4.5c-.7 2 1.3 4.3 4.3 4.9 2.6 1 5.6 0 6.2-2s-1.3-4.3-4.3-5.2c-2.6-.7-5.5 .3-6.2 2.3zm44.2-1.7c-2.9 .7-4.9 2.6-4.6 4.9 .3 2 2.9 3.3 5.9 2.6 2.9-.7 4.9-2.6 4.6-4.6-.3-1.9-3-3.2-5.9-2.9zM244.8 8C106.1 8 0 113.3 0 252c0 110.9 69.8 205.8 169.5 239.2 12.8 2.3 17.3-5.6 17.3-12.1 0-6.2-.3-40.4-.3-61.4 0 0-70 15-84.7-29.8 0 0-11.4-29.1-27.8-36.6 0 0-22.9-15.7 1.6-15.4 0 0 24.9 2 38.6 25.8 21.9 38.6 58.6 27.5 72.9 20.9 2.3-16 8.8-27.1 16-33.7-55.9-6.2-112.3-14.3-112.3-110.5 0-27.5 7.6-41.3 23.6-58.9-2.6-6.5-11.1-33.3 2.6-67.9 20.9-6.5 69 27 69 27 20-5.6 41.5-8.5 62.8-8.5s42.8 2.9 62.8 8.5c0 0 48.1-33.6 69-27 13.7 34.7 5.2 61.4 2.6 67.9 16 17.7 25.8 31.5 25.8 58.9 0 96.5-58.9 104.2-114.8 110.5 9.2 7.9 17 22.9 17 46.4 0 33.7-.3 75.4-.3 83.6 0 6.5 4.6 14.4 17.3 12.1C428.2 457.8 496 362.9 496 252 496 113.3 383.5 8 244.8 8zM97.2 352.9c-1.3 1-1 3.3 .7 5.2 1.6 1.6 3.9 2.3 5.2 1 1.3-1 1-3.3-.7-5.2-1.6-1.6-3.9-2.3-5.2-1zm-10.8-8.1c-.7 1.3 .3 2.9 2.3 3.9 1.6 1 3.6 .7 4.3-.7 .7-1.3-.3-2.9-2.3-3.9-2-.6-3.6-.3-4.3 .7zm32.4 35.6c-1.6 1.3-1 4.3 1.3 6.2 2.3 2.3 5.2 2.6 6.5 1 1.3-1.3 .7-4.3-1.3-6.2-2.2-2.3-5.2-2.6-6.5-1zm-11.4-14.7c-1.6 1-1.6 3.6 0 5.9 1.6 2.3 4.3 3.3 5.6 2.3 1.6-1.3 1.6-3.9 0-6.2-1.4-2.3-4-3.3-5.6-2z"/>
</svg>
</a>

</div>

---
layout: intro
transition: slide-left
---

<div  style="margin-top:-5mm">
<h1> Current computers </h1> can be broadly said to have two main processing units:
<span v-click>
<h2 color="green">CPU</h2>s (central processing units)
</span>
<span v-click>
and
<h2 color="purple">GPU</h2>s (graphical processing units)
</span>
</div>

<div class="grid grid-cols-[1%_40%_10%_40%] gap-3">
  <div></div>
  <div v-click style="margin-top:5mm;margin-left:-0mm; ">
  <p style="font-size:0.7em; line-height: 1.0em;">
    <span class="font-bold" color="green">CPU</span>s: for <b>OS</b>, <b>computation</b>, <b>general applications</b>. <br><span class="font-bold" color="purple">GPU</span>s: dedicated just for <b>video</b> and <b>graphics</b> applications.
  </p>
<ImageFigure
  src="/Consumer_PC.png"
  caption="A consumer machine"
  width="80%"
  height="auto"
/>

  </div>
  <div></div>
  <div v-click style="margin-top:5mm;margin-left:-0mm;">
  <p style="font-size:0.7em; line-height: 1.0em;margin-bottom:12mm;">
    Current (heterogeneous) clusters have both <span class="font-bold" color="green">CPU</span>s and <span class="font-bold" color="purple">GPU</span>s for <b>computations</b>.

  </p>
  <br>

<ImageFigure
  src="/Cluster_Hardware.png"
  caption="A typical heterogeneous computing cluster"
  width="90%"
  height="auto"
/>
  </div>
</div>


<style>
h1 {
  display: inline;
}
h2 {
  display: inline;
}
</style>

---
layout: intro
transition: none
---

# Why use GPUs for lattice?

Lattice points independently computed & updated 
      <mdi-arrow-right/> Limit of threads is number of lattice sites!

<v-click>
<div class="grid grid-cols-[25%_50%_25%] gap-3">
  <div style="margin-top:13mm;margin-left:-0mm;">
  <small>
  
-  AMD EPYC 7763: **64**
-  Intel Xeon 6148 (Skylake): **20**
-  AMD Ryzen 9 7945HX: **16**

</small>
  </div>
  <div>

|      |  CPU | GPU  |
| ---- | ---- | ---- |
| Cores/Node | $\mathcal{O}(10-100)$ | $\mathcal{O}(10000)$ |
| Clock speed  | ~ 3 GHz | ~ 1.5 GHz |

  </div>
  <div style="margin-top:13mm;">
  <small>
  
-  Nvidia H100: **~15000**
-  Nvidia RTX4070m: **~5000**

</small>
  </div>
</div>
</v-click>

<par>&nbsp;</par>

<v-click>

**CPU**: &nbsp; Low parallelization, high clock speed

**GPU**: &nbsp; High parallelization, moderate clock speed

&nbsp; <mdi-arrow-right/> CosmoLattice on GPUs has the potential for *massive parallelism* with $\gg \mathcal{10^5}$ simultaneous operations.
</v-click>
&nbsp;

---
layout: intro
transition: slide-left
---

# Why use GPUs for lattice?

Lattice points independently computed & updated 
      <mdi-arrow-right/> Limit of threads is number of lattice sites!

<div class="grid grid-cols-[25%_50%_25%] gap-3">
  <div>
  </div>
  <div>

|      |  CPU | &nbsp; &nbsp;&nbsp; GPU  |
| ---- | ---- | ---- |
| Cache/Thread | 64KB / 16MB       | &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; 1KB                 |
| Local Cache  | 64MB       | 256KB / 50MB                 |

  </div>
</div>

<div class="grid grid-cols-[45%_43%] gap-3" style="margin-top:4mm">
  <div>

**CPU**: &nbsp; <span class="font-bold" color="green">Thread-constrained</span>

**GPU**: &nbsp; <span class="font-bold" color="violet">Memory-constrained</span>
  </div>
  <div>
<ImageFigure
  src="/Arch_overview_GPU_CPU.png"
  caption="Hardware layouts [docs.nvidia.com]"
  width="100%"
  height="auto"
/>
  </div>
</div>


&nbsp;

---
layout: default
transition: fade
---

# Redesigning TempLat for GPUs

Device-centric programming.



<div class="grid grid-cols-[25%_48%_25%] gap-5">
  <div>

&nbsp;

<v-click>
<span v-mark="{ at: 1, color: 'green', type: 'underline' }">
Sequential work,
</span>

- expression construction
- program flow logic
- device dispatch.
<FancyArrow x1="120" y1="180" x2="320" y2="190" color="green" arc="0.2"/>
</v-click>

  </div>
  <div>
<ImageFigure
  src="/Device_Host.png"
  caption=""
  width="100%"
  height="auto"
/>
  </div>
  <div>

&nbsp;

<v-click>
<span v-mark="{ at: 2, color: 'purple', type: 'underline' }">
Parallel (grid-based) work,
</span>

- time evolution
- measurements
- memory reordering.
<FancyArrow x1="780" y1="180" x2="660" y2="190" color="purple" arc="-0.2"/>
</v-click>

  </div>
</div>

<div v-click="[1, 5]" style="margin-top:-10mm;">

<div class="grid grid-cols-[1%_37%_17%_35%] gap-5" style="margin-top:-10mm;" >
  <div>  </div>
  <div> 

 $$
 \textrm{expression} = \pi(t) + dt \cdot \Delta \phi(t)
 $$ 

<v-click at=3>

 ```cpp
auto functor = DEVICE_LAMBDA(device::IdxArray<NDim> idx, 
                              double& max) {
  // ...
};
double maximum = 0.;
 ```
 </v-click>

  </div>
  <div>
  <div style="margin-left:8mm; margin-top:4mm;"><small> Time evolution </small></div>
  <div style="margin-left:12mm; margin-top:4mm;" v-click=3><small> Maximum </small></div>
  </div>

  <div> 
<v-click at=2>

 $$
 \pi(t+dt) = \textrm{expression}
 $$ 
 </v-click>

<v-click at=4>

```cpp
device::iteration::reduce("Maximum", functor, maximum);
```
 </v-click>
  </div>
</div>
</div>

<div class="grid grid-cols-[3%_32%_28%_35%] gap-5" style="margin-top:-37mm;" >
  <div>  </div>
  <div>
<v-click at=5>

  **Standard C++ on CPU**
</v-click>
  </div>

  <div>

  <v-click at=6>

  **Backends**

  <small>

-  *Kokkos*
-  *Sequential STL (2020/2023)*
-  ...

  </small>
  </v-click>

  </div>
  <div>

<v-switch at=6>
<template #0>

  **Hardware-dependent**

<small>

  - NVIDIA: CUDA
  - AMD: ROCM
  - Intel: SYCL
  - shared-memory CPUs
  - FPGPAs

</small>
</template>

<template #1>

  **Abstracted away in TempLat**

<small>

  - `device::iterate::foreach`
  - `device::iterate::reduce`
  - `device::memory::copyHostToDevice`
  - ...

</small>
</template>

</v-switch>

  </div>
</div>

<style>
div {
  vertical-align: top;
  margin-bottom: 0px;
  margin-bottom: 0px;
  margin-top: 0px;
}
img {
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 0px;
  margin-top: 0px;
  padding: 0px 0;
  width: 75%;
}
.slidev-code {
  font-size: 0.5em !important;
  line-height: 0.4em !important;
  padding: 1em !important;
}
</style>


---
layout: default
transition: fade
zoom: 0.8
---

# Does this make CosmoLattice harder to use?

<v-click>

# No, but...
</v-click>

<div class="grid grid-cols-[50%_50%] gap-3">

<div>
<v-click at=2>
<br>
<div style="text-align: center">
 <h2>Model file</h2>
</div>
<br>

````md magic-move {at:3, lines:true}

```cpp
  public:

    MODELNAME(ParameterParser &parser, RunParameters<double> &runPar, 
        std::shared_ptr<MemoryToolBox> toolBox)
      ...
```

```cpp
  public:
    static constexpr size_t NDim = Model<MODELNAME>::NDim;

    MODELNAME(ParameterParser &parser, RunParameters<double> &runPar, 
        std::shared_ptr<MemoryToolBox<NDim>> toolBox)
      ...
```
````

</v-click>

<v-click at=6>
<br>
<div style="text-align: center">
 <h2>Building</h2>
</div>
<br>

<v-switch at=6>
<template #1>
Cosmolattice up to now:
</template>
<template #2>
New version: CUDA is detected automatically:
</template>
<template #3>
Granular control: shared memory OpenMP through Kokkos
</template>
</v-switch>

````md magic-move {at:7, lines:true}

```bash
$ cmake .. -DMODEL=lphi4
...
```

```bash
$ cmake .. -DMODEL=lphi4
...
-- Device configuration: 
    CUDA: ON 
    HIP: OFF 
    OpenMP: OFF 
    Threads: OFF
-- Using Kokkos as device provider.
...
```

```bash
$ cmake .. -DMODEL=lphi4 -DDEVICE=KOKKOS -DCUDA=OFF
...
-- Device configuration: 
    CUDA: OFF 
    HIP: OFF 
    OpenMP: ON 
    Threads: OFF
-- Using Kokkos as device provider.
...
```

````

</v-click>



</div>

<div>
<v-click at=4>
<br>
<div style="text-align: center">
 <h2>TempLat</h2>
</div>
<br>

````md magic-move {at:5, lines:true}

```cpp
vType computeConfigurationSpace() {
  vType localResult{};

  auto& it = mT.getToolBox()->itX();
  for(it.begin();it.end();++it)
  {
    const ptrdiff_t i = it();
    localResult += GetValue::get(mT,i);
  }

  return mWorkspace;
}
```

```cpp
vType computeConfigurationSpace() {
  auto functor = DEVICE_CLASS_LAMBDA(const device::IdxArray<NDim> &idx, 
                                     vType &update) {
    device::apply([&](const auto &...args) {
          update += GetValue::get(mT, args...);
        },
        idx);
  };

  vType localResult{};
  device::iteration::reduce("Averager", cLayout, functor, localResult);
  return localResult;
}
```
````
</v-click>
</div>


</div>

---
layout: fact
transition: slide-left
---

## For "average" user: 
&nbsp; &nbsp; 
<h1> Only minimal changes &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</h1>

&nbsp;


---
layout: default
transition: fade
---


<div class="grid grid-cols-[46%_15%_40%] gap-3">
  <div>

# GPU architecture
GPU thread hierarchy
<div v-click>

-  GPUs have $\mathcal{O}(10)$ *Streaming multiprocessors*.
</div>
<div v-click style="font-size:0.8em; line-height:1.1em; margin-left:5mm">

   -  SMs execute *kernels* with series of parallel instructions.
   -  SMs schedule their execution.
</div>
<div v-click style="margin-top:5mm">

- Work given to SMs in *blocks*.
</div>
<div v-click style="font-size:0.8em; line-height:1.1em; margin-left:5mm">

   -  A block has to fit onto a SM's hardware capabilities (~1024 threads/block).
   -  Each block's (sub-)contexts are persistent throughout its execution.
</div>
<div v-click style="margin-top:5mm">

- Internally, blocks are subdivided into *warps*.
</div>
<div v-click style="font-size:0.8em; line-height:1.1em; margin-left:5mm">

   - Each warp runs a single instruction in a *kernel* in parallel.
   - Warp size is always 32 for Nvidia, 32 or 64 for AMD.
</div>

</div>
  <div>
</div>

<div style="margin-left:-30mm;margin-top:0mm">

<v-switch at=0>
<template #0-1>
<img src="/GPU_Arch_Work_0.png">
</template>
<template #1-3>
<img src="/GPU_Arch_Work_1.png">
</template>
<template #3-5>
<img src="/GPU_Arch_Work_2.png">
</template>
<template #5-7>
<img src="/GPU_Arch_Work.png">
</template>
</v-switch>
</div>
</div>

---
layout: default
transition: fade
---

<div class="grid grid-cols-[60%_40%] gap-3">
  <div>

# Memory access patterns
Coalescing vs. sequential access

Example: Solving massless Klein-Gordon equation in $d=3$,
$$
\begin{aligned}
\partial_t^2 \phi(t, x) = \Delta \phi(t, x)\,.
\end{aligned}
$$

- Calculation of 1 thread at <span class="font-bold" color="red">red site</span>.
- <span class="font-bold" color="blue">Blue sites</span> dependents for lattice Laplacian.
<v-click at = 2>

- Memory is ordered row-major.
</v-click>
<v-click>

<par>&nbsp;</par>

### What is the optimal way to iterate over sites?

<par>&nbsp;</par>

</v-click>
</div>

<v-switch at=0>
<template #0>
<img src="/Grid_stencil_nogrid.png"/>
</template>
<template #2>
<img src="/Stencil_num.png"/>
</template>
<template #3>
<img src="/Stencil_num_CPU.png"/>
</template>
<template #4>
<img src="/Stencil_num_GPU.png"/>
</template>
<template #5>
<div style="width:560px;height:auto; position:fixed; margin-left:9mm; margin-top:9mm"><img src="/warptext.png" style="width:50px;height:auto;"></div>
<div style="width:560px;height:auto; position:fixed; margin-left:9mm; margin-top:25mm"><img src="/warptext.png" style="width:50px;height:auto;"></div>
<div style="width:560px;height:auto; position:fixed; margin-left:9mm; margin-top:40mm"><img src="/warptext.png" style="width:50px;height:auto;"></div>
<div style="width:560px;height:auto; position:fixed; margin-left:9mm; margin-top:55mm"><img src="/warptext.png" style="width:50px;height:auto;"></div>
<div style="width:560px;height:auto; position:fixed; margin-left:9mm; margin-top:71mm"><img src="/warptext.png" style="width:50px;height:auto;"></div>
<img src="/Stencil_num_GPU_tot.png"/>
</template>
</v-switch>

</div>

<div  style="margin-top:-5mm;">
</div>
<v-click at=3>

**CPU**:
 <span class="font-bold" color="green">Sequential access pattern</span> allows for caching of subsequent operations of a single thread.
</v-click>

<v-click at=4>

**GPU**: 
<span class="font-bold" color="violet">Coalesced access pattern</span> allows for simultaneous reading of memory for multiple threads.
</v-click>

<div  style="margin-top:-1mm;">
<v-click at=5>
<small>
&nbsp; &nbsp; &nbsp;  This is similar to vectorization on a CPU! 
<br>
&nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp;  
<strong>SIMD</strong> (Single Instruction, Multiple Data) vs <strong>SIMT</strong> (Single Instruction, Multiple Threads)
</small>
</v-click>
</div>
 

---
layout: default
transition: slide-left
---

<div class="grid grid-cols-[60%_40%] gap-3">
  <div>

# Memory access patterns
Coalescing vs. sequential access

Example: Solving massless Klein-Gordon equation in $d=3$,
$$
\begin{aligned}
\partial_t^2 \phi(t, x) = \Delta \phi(t, x)\,.
\end{aligned}
$$

- Calculation of 1 thread at <span class="font-bold" color="red">red site</span>.
- <span class="font-bold" color="blue">Blue sites</span> dependents for lattice Laplacian.
- Memory is ordered row-major.

**CPU**: Prefer <span class="font-bold" color="green">prefer row-major access pattern</span>.

**GPU**: Prefer <span class="font-bold" color="violet">column-major access pattern</span>.

</div>

<img src="/Stencil.png"/>

</div>

---
layout: statement
transition: fade
---

# How does this perform in vivo?


---
layout: default
transition: fade
zoom: 0.8
---

````md magic-move {lines: true}
```cpp
#define FORCE_ACCESS_PATTERN 0 // or 1
...

int main(int argc, char **argv)
{
  constexpr size_t NDim = 3;
  using T = double;
  constexpr size_t nGrid = 512;
  constexpr size_t nGhost = 1;
  constexpr size_t nSteps = 512;
  constexpr T dt = 0.01;
  ...
  Field<NDim, T> phi("phi", toolBox);
  Field<NDim, T> pi("pi", toolBox);

  Benchmark bench([&](Benchmark::Measurer &measurer) {
    phi.inFourierSpace() = RandomGaussianField<NDim, T>("Rand", toolBox);
    pi.inFourierSpace() = RandomGaussianField<NDim, T>("Rand2", toolBox);

    for (size_t i = 0; i < nSteps; ++i) {
      pi.updateGhosts();
      device::iteration::fence();
      measurer.measure("timestepping", [&]() {
        pi = pi + dt * LatticeLaplacian<NDim, decltype(phi)>(phi); // kick
        phi = phi + dt * pi;                                       // drift
        device::iteration::fence();
      });
    }
  });

  bench.run(1);
  sayMPI << bench;
  bench.log("access_pattern_0");
}
```

```cpp {6,7}
...
    for (size_t i = 0; i < nSteps; ++i) {
      pi.updateGhosts();
      device::iteration::fence();
      measurer.measure("timestepping", [&]() {
        pi = pi + dt * LatticeLaplacian<NDim, decltype(phi)>(phi); // kick
        phi = phi + dt * pi;                                       // drift
        device::iteration::fence();
      });
    }
```
````

<v-click at=1>

<div class="grid grid-cols-[2%_52%_48%] gap-4">
<div></div>
<div style="margin-top:20mm;">

# Running this on my PC:
<br>
&nbsp;&nbsp;<h2 color="purple">GPU</h2>: NVIDIA 4070RTX mobile - 4788 Cores @ 2.175 GHz<br>
<br>
<br>
&nbsp;&nbsp;<h2 color="green">CPU</h2>: Ryzen 9 7945HX - 16 Cores @ 5.4GHz

</div>

<div>
<br>
<img src="/coalesced_sequential.png" style="width:450px;height:auto;">
</div>
</div>
</v-click>

<style>
h2 {
  display: inline;
}
.katex {
    font-size: 1.0em;
}
</style>


---
layout: default
transition: fade
zoom: 0.8
---

# Taking a closer look

<div class="grid grid-cols-[52%_48%] gap-4">
<div>

```cpp
  ...
  Benchmark bench([&](Benchmark::Measurer &measurer) {
    measurer.measure("x->k fourier", [&]() {
      phi.getMemoryManager()->confirmFourierSpace();
      pi.getMemoryManager()->confirmFourierSpace();
    });

    measurer.measure("initialize field", [&]() {
      phi.inFourierSpace() = RandomGaussianField<NDim, T>("Hoi", toolBox);
      pi.inFourierSpace() = RandomGaussianField<NDim, T>("Hai", toolBox);
    });

    measurer.measure("k->x fourier", [&]() {
      phi.getMemoryManager()->confirmConfigSpace();
      pi.getMemoryManager()->confirmConfigSpace();
    });

    for (size_t i = 0; i < nSteps; ++i) {
      measurer.measure("ghosts", [&]() {
        pi.updateGhosts();
        device::iteration::fence();
      });
      measurer.measure("timestepping", [&]() {
        pi = pi + dt * LatticeLaplacian<NDim, decltype(phi)>(phi);
        phi = phi + dt * pi;
        device::iteration::fence();
      });
    }
  });
  ...
```
</div>

<div>

<v-click>
<div style="height:380px">
<ImageFigure
  src="/split_0.png"
  height="90%"
  width="auto"
/>
</div>
<div style="margin-left:20mm; margin-top:-6mm; font-size:0.6em">CPU on MPI, GPU on CUDA</div>
</v-click>
<div style="margin-left:10mm;">

</div>

<div style="margin-left:10mm;margin-top:5mm">
<v-click at=2>

-  Fourier transformation: `cuFFT`

<br>

<div style="margin-left:10mm; margin-top:-8mm; font-size:0.7em">(automatic switch to GPU native FFTs)</div>
</v-click>
<v-click at=3><br>

-  Much faster random numbers: **Philox** algorithm.
<span style="font-size:0.5em"><br>\[Salmon et al., "Parallel random numbers: As easy as 1, 2, 3"\]
</span>

<br>

<div style="margin-left:10mm; margin-top:-18mm; font-size:0.7em">(<i>counter-based</i>, deterministic, stateless and fully parallel)</div>
</v-click>
</div>
</div>
</div>

---
layout: default
transition: fade
---

# Benchmarking $\phi^4$-theory
<span style="position:relative;top:-12px;">with the `lphi4` model in CosmoLattice</span>

<div id="overlay" style="margin-left:25mm;margin-top:12mm" v-click>
preliminary
</div> 

<div class="grid grid-cols-[30%_30%_30%] gap-4">
<div v-click><ImageFigure
  src="/bench_128.png"
  height="90%"
  width="auto"
/></div>

<div v-click><ImageFigure
  src="/bench_256.png"
  height="90%"
  width="auto"
/></div>

<div v-click><ImageFigure
  src="/bench_512.png"
  height="90%"
  width="auto"
/></div>
</div>

<div class="grid grid-cols-[25%_43.2%_30%] gap-4" style="margin-top:-20mm;">

<div></div>

<div v-click at=4>
```ts
┌───────┬─────────────┬─────────────┬─────────────┬──────────────────┐
│       │        CUDA │      OpenMP │         MPI │ speedup CUDA/MPI │
│       │ runtime [s] │ runtime [s] │ runtime [s] │           factor │
├───────┼─────────────┼─────────────┼─────────────┼──────────────────┤
│ N=128 │         2.6 │         6.9 │         7.3 │              2.8 │
│ N=256 │        19.5 │       164.9 │       140.4 │              7.2 │
│ N=512 │       283.1 │      1820.6 │      2224.7 │              7.9 │
└───────┴─────────────┴─────────────┴─────────────┴──────────────────┘
```
</div>

<div style="font-size:0.7em">
<br>
<br>
<br>
<v-click at=5>
<br>
Slightly unfair comparison <br>
(my CPU is "stronger")
</v-click>
</div>

</div>

<style>
.slidev-code {
  font-size: 0.5em !important;
  line-height: 0.4em !important;
  padding: 0.3em !important;
}
#overlay {
position: fixed; /* Sit on top of the page content */
  z-index: 2; /* Specify a stack order in case you're using a different order for other elements */
  cursor: pointer; /* Add a pointer on hover */

  font-size: 100px;
  text-transform: uppercase;
  font-family: "Oswald", sans-serif;
  transform: rotate(15deg)
             skew(-15deg);
  color: rgba(1,1,1,0.2)
}
</style>

---
layout: default
transition: slide-left
---

# Benchmarking $\phi^4$-theory 
<span style="position:relative;top:-12px;">with the `lphi4` model in CosmoLattice</span>

<div id="overlay" style="margin-left:25mm;margin-top:12mm">
preliminary
</div> 

<div class="grid grid-cols-[50%_50%] gap-4" style="margin-top:-6mm">
<div v-click style="margin-left:15mm"><ImageFigure
  src="/av_512.png"
  height="92%"
  width="auto"
/></div>
<div v-click style="margin-left:15mm"><ImageFigure
  src="/V_512.png"
  height="92%"
  width="auto"
/></div>
</div>

<div style="position:absolute; top:120px; left:435px">

$N=512$
</div>

<div v-click style="margin-top:-18mm;margin-left:55mm"><ImageFigure
  src="/dE_512.png"
  height="75%"
  width="auto"
/></div>

<style>
.slidev-code {
  font-size: 0.5em !important;
  line-height: 0.4em !important;
  padding: 1em !important;
}
#overlay {
position: fixed; /* Sit on top of the page content */
  z-index: 2; /* Specify a stack order in case you're using a different order for other elements */
  cursor: pointer; /* Add a pointer on hover */

  font-size: 100px;
  text-transform: uppercase;
  font-family: "Oswald", sans-serif;
  transform: rotate(15deg)
             skew(-15deg);
  color: rgba(1,1,1,0.2)
}
</style>

---
layout: default
transition:
---

<div class="grid grid-cols-[45%_15%_40%] gap-3">
  <div>

# Scaling it up

Using large GPU clusters

-  To use large clusters and link up many nodes, CosmoLattice uses the **Message-Passing Interface (MPI)** (see lecture yesterday).

-  Send data in RAM (e.g. ghosts) between neighbouring nodes.

<v-click>
<div style="margin-bottom:2mm; margin-left:12mm">

### What about MPI+GPUs?
</div>

</v-click>

<v-click style="margin-top:5mm">

-  **GPU-aware MPI** can exchange data directly between device memory.

  <div style="margin-left:8mm; margin-top:-0mm; font-size: 0.8em">
  Support since before 2013: 

  - OpenMPI
  - MVAPICH2
  - Cray MPI
  - IBM MPI
  
  </div>

-  No changes in MPI-code!

</v-click>

</div>
<div>
</div>

<div style="margin-left:-30mm;margin-top:0mm">

<v-switch at=0>
<template #0-2>
<img src="/MPI+GPU_0.png">
</template>
<template #2-3>
<img src="/MPI+GPU_1.png">
</template>
</v-switch>
</div>
</div>

---
layout: end
---

# Questions?

Thanks for your attention!

Release of CosmoLattice with GPUs ~ early 2026