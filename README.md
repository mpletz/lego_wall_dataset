# Lego wall dataset

Contribution of the [KKV](https://www.kunststofftechnik.at/konstruieren) to the module _Digital Skills in Polymer Science_ that starts in the winter term 2025/26.

## Problem description

A lego wall is hit by a rigid sphere with a constant velocity of 20 m/s in a [scripted FEM model](https://www.github.com/mpletz/brickfem):

<div align="center">
<img src="images/fem_results/random-fem-design/random-017-000_6ms-anim.gif" width="350">
</div>

During and after the impact, work $E_{wk}$ is done in the model, the kinetic energy $E_{kin}$ is increased, and frictional dissipation $E_{fric}$ occurs. 

<div align="center">
<img src="images/fem_results/random-fem-design/random-017_res.png" width="500">
</div>

It the aim of the wall is to stop a projectile or slow it down as much as possible, these energies should be maximized. Since converting the kinetic energy of the sphere to kinetic energy of the Lego bricks may not be the best option, the frictional dissipation $E_{fric}$ at the end of the simulation can be regarded as the most relevant output.

### Results for four simple designs

<div align="center">
<img src="images/fem_results/singles-expl-mesh075mm/singles-000_6ms-anim.gif" width="200">
<img src="images/fem_results/full-expl-mesh075mm/full-000_6ms-anim.gif" width="200">
<img src="images/fem_results/only-1x4-expl-mesh075mm/only-1x4-000_6ms-anim.gif" width="200">
<img src="images/fem_results/alternate-1x4-expl-mesh075mm/alternate-1x4-000_6ms-anim.gif" width="200">
</div>

<div align="center">
<img src="images/fem_results/singles-expl-mesh075mm/singles_res.png" width="350">
<img src="images/fem_results/full-expl-mesh075mm/full_res.png" width="350">
</div>

<div align="center">
<img src="images/fem_results/only-1x4-expl-mesh075mm/only-1x4_res.png" width="350">
<img src="images/fem_results/alternate-1x4-expl-mesh075mm/alternate-1x4_res.png" width="350">
</div>

## Details on the dataset

A dataset of 5281 lego wall designs (width of 8 studs and height of 7 bricks) with the resulting $E_{wk}$, $E_{kin}$, and $E_{fric}$ is provided as a `json` file. The models were run in a previous optimization with 11 different start designs. The design is defined as described in the following. Two study next to each other can either belong to the same brick (1 in the design list) or to different bricks (0 in the design list). For each row, this means that a list of elements 0 or 1 with the length 7 can define the bricks used. The whole design list is then a list of seven such lists, starting from the top:

![alt text](images/design_def.png)

Since the dataset only considers symmetrical designs (which means that in total, 2$^4$ = 16 designs are possible per row, which means that a total of 16$^7$ = 268,435,456 designs are possible), each row starts from the center intersection and is therefore only a 4x7 list:

![alt text](images/design_sym.png)

The dataset then contains the `wall_right` list and the energies (in Nmm) at the end of the simulation:

```
"1": {"wall_right": [[1, 0, 1, 1], [1, 0, 1, 0], [0, 1, 0, 1], [0, 0, 0, 1], [0, 0, 1, 0], [1, 0, 1, 0], [1, 0, 1, 1]], "E_fric": 990.3816, "E_kin": 1202.9989, "E_wk": 2451.2402},
...
```

## Aim of the analysis

Develop a model to predict $E_{wk}$, $E_{kin}$, and $E_{fric}$ from any `wall_right` lists. Can you suggest a design with a larger $E_{fric}$ than occurring in the dataset?