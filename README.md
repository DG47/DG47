# Dhruv Gupta

This month I am loading Medicare CCLF claim files into Snowflake, running them through the [Tuva Project](https://github.com/tuva-health/tuva-core) into OMOP-shaped tables, and [arguing with a vocabulary](https://github.com/OHDSI/Vocabulary-v5.0/issues/1311) about whether sodium oxybate is an ingredient. That is the job. I am a founding engineer at [Sparx](https://usesparx.com), where I build the claims data platform under our reinsurance and stop-loss analytics for value-based care providers and health plans.

Most of the bugs I hit at work turn out to live in somebody else's repository, so I send the fix there.

## The platform

Sparx takes raw claims and turns them into the numbers an actuary or a reinsurer will sign off on. My part of that:

- **Ingestion.** CMS CCLF files for Medicare ACOs, plus commercial and Medicare Advantage claim and eligibility feeds, each in its own layout, landed into Snowflake with the layout checked before anything downstream runs.
- **Modeling.** A dbt pipeline built on the Tuva Project's CCLF connector that normalizes claims, eligibility and providers into a common layer, scheduled weekly in Airflow across a few dozen client databases.
- **Reporting.** Stop-loss and high-cost-claimant reports, member months, and the reconciliations that go back to the data source when a total does not tie out.
- **Data quality.** Checks on every load, and the OHDSI Data Quality Dashboard where the data is OMOP-shaped. When a check is wrong rather than the data, that becomes an upstream pull request.

The platform is private. The fixes it generates mostly are not.

## Upstream

Merged pull requests, grouped by what they touch.

**OMOP and the OHDSI stack**
- [SqlRender: translate `ALTER TABLE ADD` to `ADD COLUMN` for Redshift, Spark and BigQuery](https://github.com/OHDSI/SqlRender/pull/404), plus [`DROP INDEX IF EXISTS`](https://github.com/OHDSI/SqlRender/pull/405) and [sub-day `DATEDIFF` on DuckDB and SQLite](https://github.com/OHDSI/SqlRender/pull/407)
- [DatabaseConnector: restore auto-commit after a failed query on JDBC connections](https://github.com/OHDSI/DatabaseConnector/pull/345), and [report Java and driver versions with SQL errors](https://github.com/OHDSI/DatabaseConnector/pull/344)
- [WebAPI: build the drug concept hierarchy from standard ingredients only](https://github.com/OHDSI/WebAPI/pull/2566), [fix cache warming when person counts are off](https://github.com/OHDSI/WebAPI/pull/2562), and [repair the CI build that was blocking every PR](https://github.com/OHDSI/WebAPI/pull/2551)
- [CommonDataModel: put VISIT_DETAIL back in the specification table of contents](https://github.com/OHDSI/CommonDataModel/pull/818)

**Claims, FHIR and synthetic data**
- [Tuva: stop expecting `observation_flag` from office visit and orphaned claim encounters](https://github.com/tuva-health/tuva-core/pull/1464), and [align model docs with the columns actually emitted](https://github.com/tuva-health/tuva-core/pull/1456)
- [Synthea: export every payer transition that claim transactions reference](https://github.com/synthetichealth/synthea/pull/1734), [fill secondary payer columns only for secondary payers](https://github.com/synthetichealth/synthea/pull/1744), and [make quality-of-life observations optional in the CSV export](https://github.com/synthetichealth/synthea/pull/1736)
- [fhir-core: accept leap seconds in `dateTime`, `instant` and `time`](https://github.com/nazrulworld/fhir-core/pull/33), and [omit empty lists when serializing](https://github.com/nazrulworld/fhir-core/pull/34)
- [fhirpath-py: fix `sum()`, `min()`, `max()` and `avg()` over resource values](https://github.com/beda-software/fhirpath-py/pull/84)

**DICOM**
- [highdicom: decode SR content items with value type WAVEFORM](https://github.com/ImagingDataCommons/highdicom/pull/473), [parse NUM items without a measured value](https://github.com/ImagingDataCommons/highdicom/pull/476), [fix `ContentSequence` item lookups](https://github.com/ImagingDataCommons/highdicom/pull/480), and [exclude context items from measurement group accessors](https://github.com/ImagingDataCommons/highdicom/pull/496)
- [pydicom: reset the private block cache on `pop`, `popitem`, `clear` and creator replacement](https://github.com/pydicom/pydicom/pull/2396)

**De-identification**
- [phileas: detect day-first month-name dates with a comma before the year](https://github.com/philterd/phileas/pull/465), [raise on an unknown split method instead of silently splitting by newline](https://github.com/philterd/phileas/pull/448), and [keep zip code truncation within 1 to 4 digits](https://github.com/philterd/phileas/pull/431)

**SQL and data tooling**
- [sqlglot: parse Snowflake `(+)` outer join markers](https://github.com/tobymao/sqlglot/pull/8492)
- [sqllineage: resolve column lineage through a subquery column alias list](https://github.com/reata/sqllineage/pull/856), and [match columns by name for `UNION ALL BY NAME`](https://github.com/reata/sqllineage/pull/863)
- [iceberg-python: stop double-encoding the parent namespace in the REST catalog](https://github.com/apache/iceberg-python/pull/4112), and [support view types when writing partitioned tables](https://github.com/apache/iceberg-python/pull/4121)
- [dbt-duckdb: quote contract column names in the insert column list](https://github.com/duckdb/dbt-duckdb/pull/856), and [match external write option names case-insensitively](https://github.com/duckdb/dbt-duckdb/pull/869)
- [dbt-clickhouse: apply `query_settings` to materialized view queries](https://github.com/ClickHouse/dbt-clickhouse/pull/755), and [support a list `unique_key` in snapshots](https://github.com/ClickHouse/dbt-clickhouse/pull/751)
- [pandera: coerce Polars columns to Decimal without a Float64 round trip](https://github.com/unionai-oss/pandera/pull/2574), and [resolve parametrized generic Decimal dtypes](https://github.com/unionai-oss/pandera/pull/2592)
- [pointblank: treat NaN as missing in comparison steps on DuckDB](https://github.com/posit-dev/pointblank/pull/474)
- [SQLMesh: look up last-modified timestamps in each table's own Snowflake database](https://github.com/SQLMesh/sqlmesh/pull/6132)
- [datahub: use the Athena operator database as the default schema in Airflow lineage](https://github.com/datahub-project/datahub/pull/20469)
- [dbt-mcp: kill the dbt subprocess when a tool call times out](https://github.com/dbt-labs/dbt-mcp/pull/920)

There is more in review at Airflow, dbt-adapters, Iceberg, Great Expectations and the OHDSI repositories.

Not all of it went cleanly. On one of the highdicom fixes I committed a reviewer's one-line suggestion straight from the browser. It called the method it was inside, four tests hit a `RecursionError`, and the version that merged is the one I wrote after running them. I now run every suggestion locally first, which should have been obvious.

## Tools I reach for

Python, SQL and R most days, Go and Java when the job calls for it, C++ when it is a camera. Snowflake, Airflow, dbt, Postgres and Redshift for data. AWS, Kubernetes, Terraform, Docker and GitHub Actions underneath. PyTorch and OpenCV from the earlier life described below.

On the side I am building Friday, a local-first voice assistant on a Waveshare ESP32-P4 touch display. The board is the face; a backend owns the conversation, routes between local and hosted models, and keeps reminders and timers alive across restarts. It is not flashed to hardware yet, and the simulator is where most of it has been tested.

## Before Sparx

Recommendation systems at BulkMagic. Healthcare ML and predictive modeling at Meditab Software and DosePacker. Real-time computer vision, SLAM and assistive robotics at the University of Pittsburgh and the Human Engineering Research Laboratories, some of which survives as [ROS plane segmentation](https://github.com/DG47/plane_segmentation). The research along the way covered tree species detection, Hindi news classification, [brain tumor detection](https://github.com/DG47/brain-tumor-detection), breast cancer prediction, and vision for assistive systems.

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

<a href="mailto:todhruvg@gmail.com">
  <img src="https://img.shields.io/badge/Email-todhruvg%40gmail.com-EA4335?style=flat-square&logo=gmail&logoColor=white" alt="Email" />
</a>
<a href="https://www.linkedin.com/in/DG47/">
  <img src="https://img.shields.io/badge/LinkedIn-Dhruv_Gupta-0A66C2?style=flat-square&logo=linkedin&logoColor=white" alt="LinkedIn" />
</a>
