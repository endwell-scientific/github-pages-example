Create an environment file from an existing conda env

From ~/scripts/conda-export/src run:

```
python -m ecoport -f environment.yaml
```


```
git clone git@github.com:LMLhub/example-repo.git
```

Create new conda environment from scratch
```
mamba create -n env-github-pages -c conda-forge python=3.14
```
or use an existing environment file:
```
 mamba env create --file environment.yaml
```

Activate new environment:
```
mamba activate env-example-repo
```
