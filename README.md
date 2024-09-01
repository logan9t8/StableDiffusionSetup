# Stable Diffusion setup in Arch Linux

## If using WSL and VRAM is low
Allocate more VRAM by configuring C:\Users\<username>\\.wslconfig
```
[wsl2]
memory=10GB
```
[WSL2 by default allocates 50% of the RAM](https://learn.microsoft.com/en-us/windows/wsl/wsl-config#main-wsl-settings)

## Setup

Install conda
```shell
yay -S miniconda3 conda-zsh-completion
echo '. /opt/miniconda3/etc/profile.d/conda.sh' | sudo tee /etc/profile.d/miniconda3.sh 1> /dev/null
sudo chmod +x /etc/profile.d/miniconda3.sh
#<Reboot>
conda config --set auto_activate_base false
sudo conda update -n base -c defaults conda
```

Install Stable Diffusion

```shell
# If VRAM is low use this fork
git clone https://github.com/basujindal/stable-diffusion
# Else use the original repo
git clone https://github.com/CompVis/stable-diffusion

cd stable-diffusion
conda env create -f environment.yaml
conda activate ldm
nvidia-smi # Check if the GPU is detected
mkdir -p models/ldm/stable-diffusion-v1/

# Download the model
curl https://www.googleapis.com/storage/v1/b/aai-blog-files/o/sd-v1-4.ckpt?alt=media > models/ldm/stable-diffusion-v1/model.ckpt
```

## Running the code
```shell
python scripts/txt2img.py --n_samples 1 --W 1080 --H 720 --prompt "Your prompt here"
# optimizedSD/optimized_txt2img.py (if using basujindal's fork)
# --precision full (Use for better quality, requires tensor cores [RTX 20 series and above])
```

## Credits
* https://stability.ai/stable-image
* https://github.com/CompVis/stable-diffusion
* https://github.com/basujindal/stable-diffusion
* https://docs.anaconda.com/miniconda/
