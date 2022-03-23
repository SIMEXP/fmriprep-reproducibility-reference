# fmriprep-reproducibility-reference
Datalad repository that holds the fuzzy and IEEE fmriprep reproducibility reference.

# Instructions

## Initial installation

1. Create a datalad instance
```
datalad create -c text2git fmriprep-reproducibility-reference
```
2. Generate reference with fmriprep-reproducibility utility
3. copy outputs/fuzzy directory from fmriprep-reproducibility utility at the root of the repo:
```
sbatch --mem=1G --time=48:00:0 --cpus-per-task=1 --output=.mv_ref_%j.out --error=.mv_ref_%j.err --wrap "rsync -arRltv --include="*_anat_*" --include="*/" fmriprep-reproducibility/outputs/fuzzy/reference/fmriprep fmriprep-reproducibility-reference/; exit 0"
```
4. Commit new files to datalad
```
datalad save -m "Initial version"
```
5. Create a github token at this page: https://github.com/settings/tokens and save it.
6. Push repository and data to osf and github
```
datalad osf-credentials
datalad create-sibling-osf \
--title "fMRIPrep reproducibility reference" \
-s osf-annex2 \
--description "A dataset that holds fMRIPrep outputs" \
--public
datalad create-sibling-github fmriprep-reproducibility-reference \
-s github \
--github-login $USER \
--github-organization SIMEXP \
--access-protocol ssh \
--publish-depends osf-annex2-storage
datalad push --to github
```
7. create the tag
```
git tag vXX.X.Xlts
git push --tags
```

## Updating data

1. generate reference with fmriprep-reproducibility utility
2. datalad clone this repository
3. cd fmriprep-reproducibility-reference
4. copy outputs/fuzzy directory from fmriprep-reproducibility utility at the root of the repo
5. datalad save -m "message" fuzzy (slurm allocation if using CC scratch: sbatch --account=rrg-XXX --mem=16G --time=48:00:0 --cpus-per-task=1 --wrap "cd $SCRATCH/fmriprep-reproducibility-reference/ && datalad save -m \"adding fuzzy dir\" fuzzy/")

