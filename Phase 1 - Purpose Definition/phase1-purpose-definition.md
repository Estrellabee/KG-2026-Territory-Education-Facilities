# Phase 1 - Purpose Definition

## Project Name
**Territory and Education Facilities**
区域与教育设施

## Purpose
The purpose of this project is to engineer a Knowledge Graph which models various geographical features and different education-related facilities within a territory (e.g., Trentino), based on a precise set of user-oriented Competency Questions (CQs) that the Knowledge Graph can answer. The scope focuses on municipalities around Trento and Rovereto, and covers local educational sites such as kindergartens, primary schools, secondary schools and higher-education institutions.

本项目的目的是构建一个知识图谱，基于一套明确的用户能力问题（CQ），建模特伦蒂诺地区（重点为特伦托与罗韦雷托周边市镇）的地理要素与教育相关设施，涵盖幼儿园、小学、中学和高等教育机构等本地教育场所。

## Persona 人物
**Anna** is a parent who plans to move to southern Trentino. Before choosing which municipality to settle in, she needs to compare the educational services available in different towns: what types of institutions exist, whether facilities are close to where she would live, how school supply varies across towns, and whether any town lacks higher-education options.

安娜是一位计划搬到南特伦蒂诺的家长。在决定定居哪个市镇之前，她需要比较不同城镇可获得的教育服务：有哪几类教育机构、设施是否就近、各镇的学校分布有何差异，以及是否有市镇缺少高等教育资源。

## Scenario 剧情
Anna is comparing municipalities near Trento and Rovereto. She explores the knowledge graph to answer: which educational institutions each town provides, which facilities lie near major towns or natural landmarks such as lakes and mountains, which areas have the most or fewest schools, and whether any municipality has no higher-education institution nearby. She uses these queries to support her family relocation decision-making.

安娜正在比较特伦托和罗韦雷托附近的市镇。她通过查询知识图谱来了解：每个城镇提供哪些教育机构；哪些设施位于主要城镇或湖泊、山地等自然地标附近；哪些地区学校最多或最少；以及是否有市镇周边没有高等教育机构。她用这些查询结果支持家庭搬迁决策。

## Competency Questions (CQs)

### Core CQs (from project proposal #6) 核心能力问题（取自课件 #6 原版）
1. Which types of educational institutions are available in a specific municipality (e.g., Trento or Rovereto)?
   某个特定市镇（如特伦托或罗韦雷托）有哪几类教育机构？
2. What educational facilities are located near major towns or natural landmarks (like lakes or mountains)?
   哪些教育设施位于主要城镇或湖泊、山地等自然地标附近？
3. Which areas in Trentino have the highest or lowest number of schools?
   特伦蒂诺哪些地区的学校数量最多或最少？
4. Are there any municipalities without access to higher education institutions?
   是否有市镇无法就近获得高等教育机构？

### Supporting queries (optional) 补充查询（可选）
5. What are the address and geographic coordinates of a selected educational facility?
   某一选定教育设施的地址和经纬度是什么？
6. Which municipalities provide kindergarten / primary-school / secondary-school facilities?
   哪些市镇提供幼儿园 / 小学 / 中学设施？

> **Open issues to verify against data**：CQ2 (near landmarks), CQ3 (highest/lowest school count) and CQ4 (no higher-education access) can only be confirmed after inspecting the source CSV fields. Any CQ that the available data cannot support will be documented transparently as an open issue rather than solved with a self-searched new data source.
> 待数据核实：CQ2（就近地标）、CQ3（学校数量最多/最少）、CQ4（无高等教育）能否成立，需在查看原始 CSV 字段后确认；现有数据支撑不了的问题，将作为 open issue 透明记录，不自行另找数据源。

## Scope 范围
**In scope 纳入**
- Municipalities around Trento and Rovereto, within the Trentino region.
- Educational institutions: kindergartens, primary schools, secondary schools, higher-education institutions.
- Geographical features: major towns, lakes, mountains, and facility addresses / coordinates.

**Out of scope 排除**
- Public-transport commuting routes, bus/train reachability, bike/walk routes (these belong to proposal #5, not this project).
- Time-series enrollment statistics, exam scores, and reviews unless provided by the assigned datasets.

## Context 上下文边界

The context is bounded on three axes before any term is fixed (Lecture 15, "Context").

| Axis 维度 | Inside 纳入 | Outside 排除 |
|---|---|---|
| Geographic 地理 | Province of Trento: the 176 municipalities of the ISTAT list; Trento (ISTAT 22205, 157.9 km²) as centre | Other provinces, national territory, cross-border regions |
| Temporal 时间 | Snapshot of the assigned tables (school year 2022/23) + the 2023 description of the University of Trento | Historical series, projections, school-year evolution |
| Domain 领域 | Administrative territory and formal education facilities (kindergarten, primary, lower/upper secondary, university) | Transport, health, social services, school performance and rankings |

本项目在确定术语之前先划定三条边界：地理上限定为 ISTAT 名单里的 176 个特伦蒂诺市镇（以特伦托 ISTAT 22205、157.9 km² 为中心）；时间上限定为发放表格对应的 2022/23 学年快照加 2023 年的特伦托大学描述；领域上限定为行政区域与正规教育设施，交通、医疗、社会服务、学校排名一律排除。时间边界正是图谱是"快照"而不是时间序列的原因：发放的四张计数表按学年发布，且只给了单一年份。

## Focus levels 焦点层级

Terms are distributed over the three focus levels of the methodology.

| Focus 层级 | Labels 标签 | Rationale 依据 |
|---|---|---|
| common 通用 | municipality, name, address, area, code | Shared by the whole administrative domain, present in all sources |
| core 核心 | school, kindergarten, primary, secondary, university, located in | The terms the scenario actually reasons about (object of the CQs) |
| contextual 语境 | lake, mountain, landmark, near | Needed to phrase CQ2 only; carries no decision weight here |

语境层术语保留在模型中，因为 CQ2 需要它们，但它们是唯一没有实例数据的分支（发放数据里没有湖泊、山地的坐标）；这一点在 Phase 1 的 Open Issues 与报告 S9 中如实记录，而不是靠删掉 CQ2 掩盖。

## Identifying vs descriptive properties 识别属性与描述属性

| Property 属性 | Subject 主体 | Kind 类别 | Role 用途 |
|---|---|---|---|
| has_istatCode | Municipality | identifying 识别 | Unique official key; municipality IRIs are minted from the normalised name and this code is carried as the check |
| has_name | Municipality, EducationalInstitution | identifying 识别 | Human-readable label; institution IRIs are minted from it |
| has_address | EducationalInstitution | identifying (weak) 弱识别 | Locates the single building; not unique province-wide |
| has_area | Municipality | descriptive 描述 | Measured attribute, never used as a key |
| has_ownershipType | EducationalInstitution | descriptive 描述 | Classification of the single institute |
| has_schoolLevel | EducationalInstitution | descriptive 描述 | Classification, redundant with the class |
| has_*Count (four counters) | Municipality | descriptive 描述 | Computed aggregation, no key value |

识别属性用于区分同名实体并参与 IRI 构造；描述属性只补充信息、绝不用于判别实体。机构名是弱标识（同名机构可能存在），因此每条机构记录都必须先挂上所属市镇（has_municipality）才能唯一落图——这也是 Phase 4 里必须先补齐机构-市镇连线的原因。

## Initial Resources 初始资源

### Knowledge resources 知识资源
- Shared project folder: https://drive.google.com/drive/folders/1P6mqXZVkrJTMq8YycwsG_p9DXrcBjyjpp?usp=sharing

### Semi-formal resources 半正式资源
- Past education KG repository (ETG datasets): https://github.com/samuelebortolotti/Education-Trentino/tree/main/Datasets/Data%20Integration/ETG

### Informal resources 非正式资源
- Comune di Trento - student sites download: https://www.comune.trento.it/Aree-tematiche/Cartografia/Download/Studentati
- Open Data Trentino - Digital university

### Schema references 本体 / 词汇参考
- VIVO ontology (LOV): https://lov.linkeddata.es/dataset/lov/vivo
- Schema.org: https://schema.org/docs/full.html
- Schema.org (LOV): https://lov.linkeddata.es/dataset/lov/vocabs/schema
- Geospatial ontology: https://github.com/subhashishhh/Geospatial-Ontology
- Time ontology (LOV): https://lov.linkeddata.es/dataset/lov/vocabs/time
- GTFS static reference: https://gtfs.org/schedule/reference/
- GTFS ontology (LOV): https://lov.linkeddata.es/dataset/lov/vocabs/gtfs
- GTFS ontology (Subhashish): https://github.com/subhashishhh/GTFSOntology
  - Note: GTFS is a transit vocabulary; it is listed in the proposal but is **not adopted** here because this project does not model commuting.
  - 注：GTFS 是交通词汇表；课件虽列出，但本项目不建模通勤，故不采用。

### Open data catalogs 开放数据目录
- Open Data Trentino: https://dati.trentino.it/
- KGE Catalog resources: https://datascientiafoundation.github.io/LiveData-KGE/datasets/

## Preliminary ER Model 初步实体关系模型

> First-pass conceptual model; it will be refined into the formal ontology in Phase 2 (Language Definition) and Phase 3 (Knowledge Definition).
> 概念级初版模型；将在 Phase 2（语言定义）与 Phase 3（知识定义）中细化为正式本体。

### Entities 实体（Classes）
| Class 类 | Description 说明 | Subclasses 子类 |
|---|---|---|
| Municipality 市镇 | A local administrative town in Trentino | — |
| EducationalInstitution 教育机构 | Any education-related facility, top-level class | Kindergarten, PrimarySchool, SecondarySchool, HigherEducationInstitution |
| Kindergarten 幼儿园 / 托儿所 | Early-childhood education site | — |
| PrimarySchool 小学 | Primary-education site | — |
| SecondarySchool 中学 | Lower / upper secondary school | — |
| HigherEducationInstitution 高等教育机构 | University or other tertiary institution | — |
| GeoFeature 地理要素 | A geographic reference point or area | Town, NaturalLandmark |
| NaturalLandmark 自然地标 | Lake, mountain, or similar natural feature | Lake, Mountain |

### Relationships 关系（Object Properties）
| Domain 主体 | Property 关系 | Range 客体 | Meaning 含义 |
|---|---|---|---|
| EducationalInstitution | has_municipality | Municipality | 设施位于某市镇 |
| EducationalInstitution | has_proximity | GeoFeature | 设施靠近某城镇 / 地标 |
| Municipality | hasEducationalInstitution | EducationalInstitution | 市镇拥有某教育机构（has_municipality 反向） |

### Data properties 数据属性
| Property 属性 | Attached to 所属实体 | Meaning 含义 |
|---|---|---|
| has_name | EducationalInstitution, Municipality, GeoFeature | 名称 |
| has_address | EducationalInstitution | 地址 |
| has_istatCode | Municipality | 市镇 ISTAT 代码 |
| has_area | Municipality | 市镇面积（km²） |
| has_ownershipType | EducationalInstitution | 公/私立/宗教（public / religious / lay） |
| has_schoolLevel | EducationalInstitution | 学校层级（kindergarten / primary / lower secondary / upper secondary） |
| has_kindergartenCount / has_primarySchoolCount / has_lowerSecondarySchoolCount / has_upperSecondarySchoolCount | Municipality | 各层级学校数量（计数表聚合） |
| latitude / longitude | EducationalInstitution, GeoFeature | 经纬度（发放数据未提供，CQ2 因此无解，记为 open issue） |

字段名一律带 `has_` 前缀，所有术语位于命名空间 `http://knowdive.disi.unitn.it/etype#`（课程要求的基础 URI）。

## CQ-to-model mapping 能力问题与模型对应
| CQ | Main entities / relationships used 主要用到的实体与关系 |
|---|---|
| CQ1 某市镇有哪类机构 | EducationalInstitution, has_municipality, has_schoolLevel, Municipality |
| CQ2 设施就近城镇 / 地标 | EducationalInstitution, has_proximity, GeoFeature, NaturalLandmark |
| CQ3 学校数量最多 / 最少 | Municipality, hasEducationalInstitution, has_*Count 计数属性聚合 |
| CQ4 无高等教育机构的市镇 | Municipality, HigherEducationInstitution, absence check |
