<img src="https://github.com/dbmi-bgm/cgap-pipeline/blob/master/docs/images/cgap_logo.png" width="200" align="right">

# CGAP Structural Variants Pipeline, Germline

This repo contains components for CGAP pipeline for structural germline variants:
 
  * CWL
  * CGAP Portal Workflows and Metaworkflow
  * ECR (Docker) source files, which allow for creation of public Docker images (using `docker build`) or private dynamically-generated ECR images (using [*cgap pipeline utils*](https://github.com/dbmi-bgm/cgap-pipeline-utils/) `deploy_pipeline`)

For more details check [*documentation*](https://cgap-pipeline-master.readthedocs.io/en/latest/Pipelines/Downstream/SV_germline/index-SV_germline.html "SV germline documentation").

### Version updates

#### v3
* Changes in repo structure to allow for compatibility with new pipeline organization
* Pipeline renamed from ``cnv`` to ``sv_germline``

#### v2
* The pipeline has been converted to work on private ECR images which are created from our public Docker images
* Various updates throughout the CGAP SV Pipeline. The current pipeline is outlined below and updates are indicated. A new version of ``granite`` (v0.1.13) is being used for steps 2-13 of the pipeline.
  * Step 1. Manta-based calling of SVs (**Update**: Manta now uses the `callRegions` flag instead of `regions`. We no longer use get_contigs.py from Parliament2 and have removed the Parliament2 github repo from the `cgap-manta:v2` Dockerfile)
  * Step 2. Granite SVqcVCF is used to count DEL and DUP variants and provide a total number of DEL and DUP variants in each sample (**New Step**)
  * Step 3. VEP/sansa annotation (**Update**: VEP now includes the `canonical` flag to identify the canonical transcript for each gene)
  * Step 4. Granite SVqcVCF is used to count DEL and DUP variants and provide a total number of DEL and DUP variants in each sample (**New Step**)
  * Step 5. Annotation filtering and SV type selection
  * Step 6. 20 Unrelated filtering (**Update**: New 20 unrelated reference file resulting from UGRP samples re-mapped with v24 of the CGAP Pipeline including alt index)
  * Step 7. Granite SVqcVCF is used to count DEL and DUP variants and provide a total number of DEL and DUP variants in each sample (**New Step**)
  * Step 8. Cytoband annotation step adds the cytoband for each breakpoint; Cyto1 and Cyto2 (**New Step**)
  * Step 9. Granite SVqcVCF is used to count DEL and DUP variants and provide a total number of DEL and DUP variants in each sample (**New Step**)
  * Step 10. Length filtering
  * Step 11. Granite SVqcVCF is used to count DEL and DUP variants and provide a total number of DEL and DUP variants in each sample (**New Step**)
  * Step 12. Annotation cleaning to produce a vcf file that loads quickly in the Higlass genome browser
  * Step 13. Granite SVqcVCF is used to count DEL and DUP variants and provide a total number of DEL and DUP variants in each sample (**New Step**)

#### v1
* Created entirely new pipeline - CGAP Structural Variant (SV) Pipeline
  * Step 1. Manta-based SV calling to generate a vcf file containing SVs in proband or trio
  * Step 2. VEP annotation for genes/transcripts and sansa annotation for gnomAD SV population allele frequencies
  * Step 3. Annotation filter to remove non-coding SVs, SVs with high allele frequency in gnomAD SV, and select for only deletions (SVTYPE=DEL) and duplications (SVTYPE=DUP)
  * Step 4. 20 Unrelated filtering to remove common and artefactual SVs compared to 20 unrelated samples that were also run through Manta
  * Step 5. Length-based filtering to remove very long variants that are likely false positives and create issues when ingested into CGAP Portal due to the size of the associated gene list
  * Step 6. Annotation cleaning to produce a vcf file that loads quickly in the Higlass genome browser
