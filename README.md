# fmriprep-reproducibility-reference
Datalad repository that holds the fuzzy and IEEE fmriprep reproducibility reference.

# Instructions

## Initial installation

1. datalad create -c text2git fmriprep-reproducibility-reference
2. generate reference with fmriprep-reproducibility utility
4. copy outputs/fuzzy directory from fmriprep-reproducibility utility at the root of the repo:
```
sbatch --mem=1G --time=48:00:0 --cpus-per-task=1 --output=.mv_ref_%j.out --error=.mv_ref_%j.err --wrap "rsync -arRltv --include="*_anat_*" --include="*/" fmriprep-reproducibility/outputs/fuzzy/reference/fmriprep fmriprep-reproducibility-reference/; exit 0"
```
5. datalad save -m "Initial version"
6. create the tag
```
git tag vXX.X.Xlts
```
7. using datald-osf, push
```
datalad osf-credentials
datalad create-sibling-osf --title "fMRIPrep reproducibility reference" \
--mode exportonly \
-s osf-export \
--description "A dataset that holds fMRIPrep outputs" \
--public
git-annex export HEAD --to osf-export-storage
```
8. Make sure to make the dataset public through the OSF portal

## Updating data

1. generate reference with fmriprep-reproducibility utility
2. datalad clone this repository
3. cd fmriprep-reproducibility-reference
4. copy outputs/fuzzy directory from fmriprep-reproducibility utility at the root of the repo
5. datalad save -m "message" fuzzy (slurm allocation if using CC scratch: sbatch --account=rrg-XXX --mem=16G --time=48:00:0 --cpus-per-task=1 --wrap "cd $SCRATCH/fmriprep-reproducibility-reference/ && datalad save -m \"adding fuzzy dir\" fuzzy/")

