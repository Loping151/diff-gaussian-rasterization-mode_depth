# Differential Gaussian Rasterization

**NOTE**: this is a modified version to support depth & alpha rendering (both forward and backward) and mode depth (without backward for now) from the [original repository](https://github.com/graphdeco-inria/diff-gaussian-rasterization). 

In addition, a floater selection function is added:

```python
def markFloater(self, positions, surface):
    # Mark points that are floating (i.e. not on the surface) with a boolean
    with torch.no_grad():
        raster_settings = self.raster_settings
        floater = _C.mark_floater(
            positions,
            surface, 
            raster_settings.viewmatrix)
```

The example definition of mode depth is here: [SparseGS]https://arxiv.org/abs/2312.00206

```python
rendered_image, radii, depth, alpha, mode_depth = rasterizer(
    means3D = means3D,
    means2D = means2D,
    shs = shs,
    colors_precomp = colors_precomp,
    opacities = opacity,
    scales = scales,
    rotations = rotations,
    cov3D_precomp = cov3D_precomp)
```

The example use of floater selection is:

```python
renderer = GaussianRasterizer(raster_settings=settings)
_, _, _, _, mode_depth = renderer(**render_var)
floater_mask = renderer.markFloater(render_var['means3D'], mode_depth) # will not check the shape of surface!
render_var['means3D'] = params['means3D'][~floater_mask]
```


Used as the rasterization engine for the paper "3D Gaussian Splatting for Real-Time Rendering of Radiance Fields". If you can make use of it in your own research, please be so kind to cite us.

<section class="section" id="BibTeX">
  <div class="container is-max-desktop content">
    <h2 class="title">BibTeX</h2>
    <pre><code>@Article{kerbl3Dgaussians,
      author       = {Kerbl, Bernhard and Kopanas, Georgios and Leimk{\"u}hler, Thomas and Drettakis, George},
      title        = {3D Gaussian Splatting for Real-Time Radiance Field Rendering},
      journal      = {ACM Transactions on Graphics},
      number       = {4},
      volume       = {42},
      month        = {July},
      year         = {2023},
      url          = {https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/}
}</code></pre>
  </div>
</section>
