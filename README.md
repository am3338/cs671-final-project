
# Intepretability of diffusion-based models for time series forecasting

## NOTE: Most of this repository is based on the following repository: https://github.com/Y-debug-sys/Diffusion-TS

## Dataset Preparation

All the four real-world datasets (Stocks, ETTh1, Energy and fMRI) can be obtained from [Google Drive](https://drive.google.com/file/d/11DI22zKWtHjXMnNGPWNUbyGz-JiEtZy6/view?usp=sharing). Please download **dataset.zip**, then unzip and copy it to the folder `./Data` in our repository. EEG dataset can be downloaded from [here](https://drive.google.com/file/d/1IqwE0wbCT1orVdZpul2xFiNkGnYs4t89/view?usp=sharing) and should also be placed in the aforementioned `./Data/dataset` folder.

## Running the Code

### Environment and libraries

Create a new environment myenv:

~~~bash
$ conda create -n myenv python=3.9
~~~

After creating the environment, run:

~~~bash
(myenv) $ pip install -r requirements.txt
~~~

### Training & Sampling

For training, you can reproduce the experimental results of all benchmarks by runing

~~~bash
(myenv) $ python main.py --name {name} --config_file Config/{name}.yaml --gpu 0 --train
~~~

Where you need to replace {name} with the dataset name (which can be one of the following: eeg, energy, etth, fmri, mujoco, sines, solar or stocks). Corresponding configuration files are provided in Config/{name}.yml. MuJoCo and Sines datasets are downloaded/generated directly by running the training script.

### Counterfactual prediction

After training the model, please run the following script for counterfactual time series forecasting:

```bash
(myenv) $ python main.py --name {dataset_name} --config_file {config.yaml} --gpu 0 --sample 1 --milestone {checkpoint_number} --mode predict-counterfactual --pred_len {pred_len}
```
For the dataset {dataset_name}, corresponding checkpoints will be saved in the Checkpoints_{name}_{pred_len} folder. For the specific checkpoint {checkpoint_number}, look for Checkpoints_{name}_{pred_len}/checkpoint-{checkpoint-number}.pt 

## Visualization and Evaluation

After sampling, synthetic data and orginal data are stored in `.npy` file format under the *output* folder, which can be directly read to calculate quantitative metrics such as discriminative, predictive, correlational and context-FID score. You can also reproduce the visualization results using t-SNE or kernel plotting, and all of these evaluational codes can be found in the folder `./Utils`. Please refer to `.ipynb` tutorial files in this repo for more detailed implementations.

**Note:** All the metrics can be found in the `./Experiments` folder. Additionally, by default, for datasets other than the Sine dataset (because it does not need normalization), their normalized forms are saved in `{...}_norm_truth.npy`. Therefore, when you run the Jupternotebook for dataset other than Sine, just uncomment and rewrite the corresponding code written at the beginning.
