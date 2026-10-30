# Dhruv Gupta

This month I am loading Medicare CCLF claim files into Snowflake, running them through the [Tuva Project](https://github.com/tuva-health/tuva-core) into OMOP-shaped tables, and [arguing with a vocabulary](https://github.com/OHDSI/Vocabulary-v5.0/issues/1311) about whether sodium oxybate is an ingredient. That is the job. I am a founding engineer at [Sparx](https://usesparx.com), where I build the claims data platform under our reinsurance and stop-loss analytics for value-based care providers and health plans: CMS and commercial claim feeds in, a Tuva-based dbt pipeline on Airflow in the middle, and the reports an actuary signs off on at the end.

Most of the bugs I hit there turn out to live in somebody else's repository, so I send the fix there.

## Upstream

A selection of merged pull requests.

**OMOP / OHDSI**
- [SqlRender: `ALTER TABLE ADD` to `ADD COLUMN` for Redshift, Spark and BigQuery](https://github.com/OHDSI/SqlRender/pull/404), [`DROP INDEX IF EXISTS`](https://github.com/OHDSI/SqlRender/pull/405), [sub-day `DATEDIFF` on DuckDB and SQLite](https://github.com/OHDSI/SqlRender/pull/407)
- [DatabaseConnector: restore auto-commit after a failed query](https://github.com/OHDSI/DatabaseConnector/pull/345)
- [WebAPI: drug hierarchy from standard ingredients only](https://github.com/OHDSI/WebAPI/pull/2566), [the CI build that was blocking every PR](https://github.com/OHDSI/WebAPI/pull/2551)

**Claims, FHIR, synthetic data**
- [Tuva: `observation_flag` on office visit and orphaned claim encounters](https://github.com/tuva-health/tuva-core/pull/1464)
- [Synthea: export every payer transition that claim transactions reference](https://github.com/synthetichealth/synthea/pull/1734)
- [fhir-core: leap seconds in `dateTime`, `instant` and `time`](https://github.com/nazrulworld/fhir-core/pull/33)
- [fhirpath-py: `sum()`, `min()`, `max()`, `avg()` over resource values](https://github.com/beda-software/fhirpath-py/pull/84)

**DICOM**
- [highdicom: WAVEFORM content items](https://github.com/ImagingDataCommons/highdicom/pull/473), [NUM items without a measured value](https://github.com/ImagingDataCommons/highdicom/pull/476), [`ContentSequence` lookups](https://github.com/ImagingDataCommons/highdicom/pull/480), [context items in measurement groups](https://github.com/ImagingDataCommons/highdicom/pull/496)
- [pydicom: private block cache after `pop`, `popitem`, `clear`](https://github.com/pydicom/pydicom/pull/2396)

**SQL and data tooling**
- [sqlglot: Snowflake `(+)` outer join markers](https://github.com/tobymao/sqlglot/pull/8492)
- [sqllineage: column lineage through a subquery alias list](https://github.com/reata/sqllineage/pull/856), [`UNION ALL BY NAME`](https://github.com/reata/sqllineage/pull/863)
- [iceberg-python: REST namespace double-encoding](https://github.com/apache/iceberg-python/pull/4112), [view types in partitioned writes](https://github.com/apache/iceberg-python/pull/4121)
- [dbt-duckdb: quote contract column names](https://github.com/duckdb/dbt-duckdb/pull/856), [dbt-clickhouse: `query_settings` on materialized views](https://github.com/ClickHouse/dbt-clickhouse/pull/755)
- [pandera: Polars Decimal without a Float64 round trip](https://github.com/unionai-oss/pandera/pull/2574), [phileas: day-first dates with a comma before the year](https://github.com/philterd/phileas/pull/465)

More is in review at Airflow, dbt-adapters, Iceberg and the OHDSI repositories. Not all of it went cleanly: on one highdicom fix I committed a reviewer's suggestion straight from the browser, it called the method it sat inside, and four tests hit a `RecursionError`. I run every suggestion locally now.

## Tools

Python, SQL and R most days, Go and Java when the job calls for it, C++ when it is a camera. Snowflake, Airflow, dbt, Postgres and Redshift for data; AWS, Kubernetes, Terraform and GitHub Actions underneath; PyTorch and OpenCV from the earlier life.

On the side: [Friday](https://github.com/DG47/friday-esp32-p4-assistant), a local-first voice assistant on an ESP32-P4 touch display. The board is the face, a backend owns the conversation and routes between local and hosted models. It runs on the physical board: voice turns, reminders and TLS re-enrollment are verified there, while speaker output and echo cancellation are still open.

## Before Sparx

Recommendation systems at BulkMagic. Healthcare ML at Meditab Software and DosePacker. Computer vision, SLAM and assistive robotics at the University of Pittsburgh and the Human Engineering Research Laboratories, a piece of which survives as [ROS plane segmentation](https://github.com/DG47/plane_segmentation). Research on tree species detection, Hindi news classification, [brain tumor detection](https://github.com/DG47/brain-tumor-detection) and breast cancer prediction.

## Education

<p>
  <img src="pitt-edu-logo.png" width="30" height="30" align="left">
  <strong>University of Pittsburgh</strong><br>
  M.S. in Information Science
</p>
<br>
<p>
  <img src="cmu-edu-logo.png" width="30" height="30" align="left">
  <strong>Carnegie Mellon University</strong><br>
  Cross-registered coursework in Computer Vision and Artificial Intelligence
</p>
<br>
<p>
  <img src="vellore-institute-of-technology-vit-logo-vector-2022.svg" width="30" height="30" align="left">
  <strong>Vellore Institute of Technology</strong><br>
  B.Tech. in Information Technology
</p>

## Outside code

- ⚽ Long-suffering, occasionally rewarded **Real Madrid** supporter on Champions League nights
- 🎬 I once made a movie: [Watch it here](https://youtu.be/S2IAEMNz15M?si=9aKyHHl7XCpn-eSz)
- 🏎️ I enjoy driving and traveling
- 📚 I tend to disappear into unnecessarily deep research rabbit holes when something interests me

## Connect

If you work with CCLF, OMOP or DICOM SR data and have run into something odd, tell me. There is a fair chance I have run into it too.

<a href="mailto:todhruvg@gmail.com"><img src="https://img.shields.io/badge/Email-todhruvg%40gmail.com-EA4335?style=flat-square&logo=gmail&logoColor=white" alt="Email" /></a>
<a href="https://www.linkedin.com/in/DG47/"><img src="https://img.shields.io/badge/LinkedIn-Dhruv_Gupta-0A66C2?style=flat-square&logo=linkedin&logoColor=white" alt="LinkedIn" /></a>
