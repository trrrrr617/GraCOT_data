# Automotive Knowledge Graph and User Intent Parsing Dataset

This open-source dataset is primarily designed for feature comparison and reasoning of automotive products based on a knowledge graph, as well as intent recognition and legality validation of user natural language queries. Built upon the AIC graph framework, the dataset includes graph database export files used for experiments and the corresponding structured test cases.

## 📂 File Directory Description

The dataset contains the following three core files:

1. **`Main.dump`**
   - **Format**: Neo4j Database Dump File.
   - **Description**: The primary data source for the experiments, containing the complete underlying graph structure and fundamental data entities.
   
2. **`BasedOnAIC.dump`**
   - **Format**: Neo4j Database Dump File.
   - **Description**: A knowledge graph built upon the AIC framework, containing specifically defined relationships, attributes, and upper-level ontology structures for this experiment.

3. **`data.json`**
   - **Format**: JSON.
   - **Description**: Contains the structured experimental data, contrastive features, and user input test cases. It is mainly divided into three parts: basic product info, feature reasoning (Exp1), and user query parsing (Exp25).

## 📊 `data.json` Data Structure Overview

`data.json` consists of the following three main modules:

### 1. `product_info` (Basic Product Information)
Contains attribute comparison data for two vehicle models (Product A and Product B) across multiple dimensions. The dimensions include, but are not limited to:
- `6万公里保养总成本预估` (Estimated Total Maintenance Cost for 60,000 km)
- `长x宽x高(mm)` (Length x Width x Height in mm)
- `车身结构` (Body Structure)
- `最高车速(km/h)` (Maximum Speed in km/h)
- `级别` (Vehicle Class)
- `整车保修期限` (Vehicle Warranty Period)

### 2. `exp1_key_features` (Feature Comparison and Reasoning)
Records the summary of high-level features inferred from product attributes. The data format is a `[Feature Description, Reasoning Basis]` list.
- **Example**: `["产品A相对产品B更耐用", "推理来自整车保修期限"]` (*"Product A is more durable than Product B", "Inferred from the vehicle warranty period"*)
- **Task Objective**: Used to verify whether models or algorithms can accurately generate or extract comparative conclusions based on the underlying graph attributes.

### 3. `exp25_user_input_and_dimensions` (User Input and Dimension Mapping)
Contains multiple user natural language queries and their corresponding parsing results, mainly used to test a model's intent recognition and boundary handling capabilities.
- `user_input`: The natural language text input by the user.
- `legal`: A boolean value determining if the input is reasonable and within the processing scope. Normal car purchase demands are `true`; unrealistic or absurd requests such as "maintenance cost under 500 yuan and speed of 500 km/h" or "100-year warranty" are marked as `false`.
- `dimensions`: If the request is legal, the corresponding graph attribute dimensions are listed here (e.g., `["长x宽x高(mm)", "级别"]`); if illegal, this value is `null`.

## 🚀 Quick Start

### Importing Neo4j Graph Data
You can use the `neo4j-admin` tool to restore the `.dump` files to your local Neo4j database. Please ensure your Neo4j version is compatible with the exported version.

```bash
neo4j stop
neo4j-admin database load main --from-path=/path/to/Main.dump
neo4j-admin database load aic_graph --from-path=/path/to/BasedOnAIC.dump
neo4j start
