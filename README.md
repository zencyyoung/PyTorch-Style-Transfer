# PyTorch-Style-Transfer

This repo provides PyTorh Implementation of **[MSG-Net (ours)](#msg-net)** and **[Neural Style (Gatys et al. CVPR 2016)](#neural-style)**. Please install [PyTorch](http://pytorch.org/) with cuda support before running the code. The code has been tested on Ubuntu 16.04 with Titan X Pascal and Maxwell.
If you would like to produce exactly the same result as in the MSG-Net paper, please visit our original [Torch implementation](https://github.com/zhanghang1989/MSG-Net/).

**Tabe of content**

* [Real-time Style Transfer using MSG-Net](#msg-net)
	- [Stylize Images using Pre-trained Model](#stylize-images-using-pre-trained-msg-net)
	- [Train Your Own MSG-Net Model](#train-your-own-msg-net-model)
* [Slow Neural Style Transfer](#neural-style)
* [Extending the Software](#extending-the-software)

## MSG-Net
<table width="100%" border="0" cellspacing="15" cellpadding="0">
	<tbody>
		<tr>
			<td>
			<b>Multi-style Generative Network for Real-time Transfer</b>  [<a href="https://arxiv.org/pdf/1703.06953.pdf">arXiv</a>] [<a href="http://computervisionrutgers.github.io/MSG-Net/">project</a>]  <br>
  <a href="http://hangzh.com/">Hang Zhang</a>,  <a href="http://eceweb1.rutgers.edu/vision/dana.html">Kristin Dana</a>
<pre>
@article{zhang2017multistyle,
	title={Multi-style Generative Network for Real-time Transfer},
	author={Zhang, Hang and Dana, Kristin},
	journal={arXiv preprint arXiv:1703.06953},
	year={2017}
}
</pre>
			</td>
			<td width="440"><a><img src ="https://raw.githubusercontent.com/zhanghang1989/MSG-Net/master/images/figure1.jpg" width="420px" border="1"></a></td>
		</tr>
	</tbody>
</table>

### Stylize Images Using Pre-trained MSG-Net
0. Download the pre-trained model
	```bash
	git clone git@github.com:zhanghang1989/PyTorch-Style-Transfer.git
	cd PyTorch-Style-Transfer/experiments
	bash models/download_model.sh
	```
0. Test the model
	```bash
	python main.py eval --content-image images/content/venice-boat.jpg --style-image images/9styles/candy.jpg --model models/9styles.model --net-type v1
	```
* If you don't have a GPU, simply set `--cuda=0`. For a different style, set `--style-image path/to/style`.
	If you would to stylize your own photo, change the `--content-image path/to/your/photo`. 
	More options:

	* `--content-image`: path to content image you want to stylize.
	* `--style-image`: path to style image (typically covered during the training).
	* `--model`: path to the pre-trained model to be used for stylizing the image.
	* `--output-image`: path for saving the output image.
	* `--content-size`: the content image size to test on.
	* `--cuda`: set it to 1 for running on GPU, 0 for CPU.

<img src ="images/1.jpg" width="260px" /> <img src ="images/2.jpg" width="260px" />
<img src ="images/3.jpg" width="260px" />
<img src ="images/4.jpg" width="260px" />
<img src ="images/5.jpg" width="260px" />
<img src ="images/6.jpg" width="260px" />
<img src ="images/7.jpg" width="260px" />
<img src ="images/8.jpg" width="260px" />
<img src ="images/9.jpg" width="260px" />

### Train Your Own MSG-Net Model
0. Download the COCO dataset
	```bash
	bash dataset/download_dataset.sh
	```
0. Train the model
	```bash
	python main.py train --epochs 4
	```
* If you would like to customize styles, set `--style-folder path/to/your/styles`. More options:
	* `--style-folder`: path to the folder style images.
	* `--vgg-model-dir`: path to folder where the vgg model will be downloaded.
	* `--save-model-dir`: path to folder where trained model will be saved.
	* `--cuda`: set it to 1 for running on GPU, 0 for CPU.

## Neural Style

[Image Style Transfer Using Convolutional Neural Networks](http://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Gatys_Image_Style_Transfer_CVPR_2016_paper.pdf) by Leon A. Gatys, Alexander S. Ecker, and Matthias Bethge.

```bash
python main.py optim --content-image images/content/venice-boat.jpg --style-image images/9styles/candy.jpg
```
* `--content-image`: path to content image.
* `--style-image`: path to style image.
* `--output-image`: path for saving the output image.
* `--content-size`: the content image size to test on.
* `--style-size`: the style image size to test on.
* `--cuda`: set it to 1 for running on GPU, 0 for CPU.

<img src ="images/g1.jpg" width="260px" />

## Extending the Software

* Extending a new network type:
	- define your own file under the folder `net/` 
	- implement your own `nn.Module` in `mynn.py` if need
	- implement train and evaluate functions base on your need
	- set up the network as in `main.py`
* Extending a new experiment (requires differet setting)
	- define the subcommand of the options in `option.py`
	- implement the experiment function like `optimize()`
	- set up the experiment as in `main.py`

### Acknowledgement
The code benefits from outstanding prior work and their implementations including:
- [Texture Networks: Feed-forward Synthesis of Textures and Stylized Images](https://arxiv.org/pdf/1603.03417.pdf) by Ulyanov *et al. ICML 2016*. ([code](https://github.com/DmitryUlyanov/texture_nets))
- [Perceptual Losses for Real-Time Style Transfer and Super-Resolution](https://arxiv.org/pdf/1603.08155.pdf) by Johnson *et al. ECCV 2016* ([code](https://github.com/jcjohnson/fast-neural-style)) and its pytorch implementation [code](https://github.com/darkstar112358/fast-neural-style) by Abhishek.
- [Image Style Transfer Using Convolutional Neural Networks](http://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Gatys_Image_Style_Transfer_CVPR_2016_paper.pdf) by Gatys *et al. CVPR 2016* and its torch implementation [code](https://github.com/jcjohnson/neural-style) by Johnson.
