---
permalink: /
title: ""
excerpt: ""
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

{% if site.google_scholar_stats_use_cdn %}
{% assign gsDataBaseUrl = "https://cdn.jsdelivr.net/gh/" | append: site.repository | append: "@" %}
{% else %}
{% assign gsDataBaseUrl = "https://raw.githubusercontent.com/" | append: site.repository | append: "/" %}
{% endif %}
{% assign url = gsDataBaseUrl | append: "google-scholar-stats/gs_data_shieldsio.json" %}

<span class='anchor' id='about-me'></span>

I am a Researcher at Alibaba Group, specializing in 3D Digital Human Reconstruction and Neural Rendering. My work spans real-time talking avatars, neural radiance fields (NeRF), and 3D Gaussian Splatting — from foundational avatar models to face-and-hair composable reconstruction and full-body articulation. My goal is to enable high-fidelity, real-time digital human synthesis for augmented reality and interactive applications.

I received my M.Eng. and B.E. from Dalian University of Technology, where I conducted my research at the [IIAU-LAB](https://iiaulab.github.io/) under the supervision of Prof. [Huchuan Lu (IEEE Fellow)](https://scholar.google.com/citations?hl=en&user=D3nE0agAAAAJ).

# 🔥 News
- *2026*: &nbsp;🎉 Paper "FHAvatar: Fast and High-Fidelity Reconstruction of Face-and-Hair Composable 3D Head Avatar" accepted to **CVPR 2026**
- *2025*: &nbsp;🎉 Paper "TaoAvatar: Real-time Lifelike Full-body Talking Avatars" accepted to **CVPR 2025 (Highlight)**
- *2024*: &nbsp;🎉 Paper "GaussianTalker: Speaker-specific Talking Head Synthesis" accepted to **ACM Multimedia 2024**

# 📝 Publications

<div class='paper-box'><div class='paper-box-image'><div><div class="badge">CVPR 2026</div><img src='images/500x300.png' alt="sym" width="100%"></div></div>
<div class='paper-box-text' markdown="1">

[FHAvatar: Fast and High-Fidelity Reconstruction of Face-and-Hair Composable 3D Head Avatar from Few Casual Captures](https://arxiv.org/abs/2603.23345)

Yujie Sun, Zhuoqiang Cai, Chaoyue Niu, **Jianchuan Chen**, Zhiwen Chen, Chengfei Lv, Fan Wu

Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2026

[**arXiv**](https://arxiv.org/abs/2603.23345)

Fast and high-fidelity reconstruction of face-and-hair composable 3D head avatars from few casual captures
</div>
</div>

<div class='paper-box'><div class='paper-box-image'><div><div class="badge">CVPR 2025 Highlight</div><img src='images/500x300.png' alt="sym" width="100%"></div></div>
<div class='paper-box-text' markdown="1">

[TaoAvatar: Real-time Lifelike Full-body Talking Avatars for Augmented Reality via 3D Gaussian Splatting](https://pixelai-team.github.io/TaoAvatar/)

**Jianchuan Chen**, Jiaxiang Hu, Guoxian Wang, Zhiheng Jiang, Tao Zhou, Zheng Chen, Changjie Lv

Proceedings of the Computer Vision and Pattern Recognition Conference, 2025

[**Project**](https://pixelai-team.github.io/TaoAvatar/) [**arXiv**](https://arxiv.org/abs/2503.17032) [**Video**](https://youtu.be/BiZQKHTSUTI) [**Dataset**](https://huggingface.co/datasets/PixelAI-Team/TalkBody4D) [**Demo**](https://github.com/alibaba/MNN/blob/master/apps/Android/Mnn3dAvatar/README.md)

Real-time lifelike full-body talking avatars for augmented reality using 3D Gaussian Splatting technology
</div>
</div>

<div class='paper-box'><div class='paper-box-image'><div><div class="badge">ACM MM 2024</div><img src='images/500x300.png' alt="sym" width="100%"></div></div>
<div class='paper-box-text' markdown="1">

[GaussianTalker: Speaker-specific Talking Head Synthesis via 3D Gaussian Splatting](https://yuhongyun777.github.io/GaussianTalker)

Hongyun Yu, Zhan Qu, Qihang Yu, **Jianchuan Chen**, Zhonghua Jiang, Zhiwen Chen, Shengyu Zhang, Jimin Xu, Fei Wu, Chengfei Lv, Gang Yu

Proceedings of the 32nd ACM International Conference on Multimedia, 3548-3557, 2024

[**Project**](https://yuhongyun777.github.io/GaussianTalker) [**arXiv**](https://arxiv.org/abs/2404.14037) [**Video**](https://www.youtube.com/watch?v=TYS-WlAchvM)

Speaker-specific talking head synthesis leveraging 3D Gaussian Splatting for realistic animation
</div>
</div>

<div class='paper-box'><div class='paper-box-image'><div><div class="badge">CVPR 2023</div><img src='images/500x300.png' alt="sym" width="100%"></div></div>
<div class='paper-box-text' markdown="1">

[GM-NeRF: Learning Generalizable Model-based Neural Radiance Fields from Multi-view Images](https://janaldochen.github.io/GM-NeRF)

**Jianchuan Chen**, Wei Yi, Lanqing Ma, Xiaoyu Jia, Hujun Lu

Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2023

[**Project**](https://janaldochen.github.io/GM-NeRF) [**arXiv**](https://arxiv.org/abs/2303.13777)

Generalizable model-based neural radiance fields learning from multi-view images
</div>
</div>

<div class='paper-box'><div class='paper-box-image'><div><div class="badge">ECCV 2022 Workshop</div><img src='images/500x300.png' alt="sym" width="100%"></div></div>
<div class='paper-box-text' markdown="1">

[Pixel2ISDF: Implicit Signed Distance Fields Based Human Body Model from Multi-view and Multi-pose Images](https://arxiv.org/abs/2212.02765)

**Jianchuan Chen**, Wentao Yi, Tiantian Wang, Xing Li, Liqian Ma, Yangyu Fan, Huchuan Lu

European Conference on Computer Vision, 366-375, 2022

[**arXiv**](https://arxiv.org/abs/2212.02765)

Human body reconstruction using implicit signed distance fields from multi-view images
</div>
</div>

<div class='paper-box'><div class='paper-box-image'><div><div class="badge">ArXiv 2021</div><img src='images/500x300.png' alt="sym" width="100%"></div></div>
<div class='paper-box-text' markdown="1">

[Animatable Neural Radiance Fields from Monocular RGB Videos](https://arxiv.org/abs/2106.13629)

**Jianchuan Chen**, Ying Zhang, Di Kang, Xuefei Zhe, Linchao Bao, Xu Jia, Huchuan Lu

arXiv preprint arXiv:2106.13629, 2021

[**ArXiv**](https://arxiv.org/abs/2106.13629) [**Github**](https://github.com/JanaldoChen/Anim-NeRF)

Animatable neural radiance fields reconstruction from monocular RGB videos
</div>
</div>

# 🎖 Honors and Awards
- **CVPR 2025 NeRSemble Benchmark Challenge**: 1st Place Winner (Dynamic Novel View Synthesis)
- **China3DV 2025 Live Demo**: Best Technical Candidate Award (Top 3)
- **CVPR 2022 Workshop SHARP Challenge**: 1st Place Winner (€4,500 prize)
- **ECCV 2022 Workshop WCPA Challenge**: 3rd Place Winner
- **ACM-ICPC Asian Regional Contests** (2017-2018): 2 Silver Medals, 2 Bronze Medals; National Invitational Silver Medal

# 📖 Educations
- **M.Eng. in Information and Communication Engineering**, Dalian University of Technology
  - Sep 2020 - Jun 2023
- **B.Eng. in Computer Science and Technology**, Dalian University of Technology
  - Aug 2016 - Jun 2020

# 💻 Internships
- **Huawei** (Jun 2022 - Oct 2022)
  - Core Network Product Line — 3D head avatar algorithms, face detection, and 3DMM-based avatar driving
- **Tencent AI Lab** (Jun 2020 - Jun 2021)
  - Virtual Human Algorithm Team — 3D digital human generation and motion transfer with NeRF + SMPL