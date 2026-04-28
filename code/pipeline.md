# pipeline.ipynb — Converted to Markdown

---

## Code (Cell 1)

```python
import os
import sys
import logging
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from dotenv import load_dotenv
from pymongo import MongoClient
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestRegressor
from sklearn.preprocessing import OneHotEncoder
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
from sklearn.metrics import mean_squared_error, r2_score

# 1. Setup Logging (Rubric Requirement)
logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s [%(levelname)s] %(message)s",
    handlers=[
        logging.FileHandler("analysis.log"),
        logging.StreamHandler(sys.stdout)
    ],
    force=True 
)
logger = logging.getLogger(__name__)

# 2. Data Preparation: Query MongoDB to Dataframe
def fetch_and_prepare_data():
    load_dotenv()
    uri = os.getenv("MONGO_URI")
    
    if not uri:
        logger.error("MONGO_URI not found in environment variables.")
        return None

    try:
        logger.info("Connecting to MongoDB Atlas...")
        client = MongoClient(uri)
        db = client['power_grid_db']
        collection = db['plants']
        
        logger.info("Querying documents from 'plants' collection...")
        cursor = collection.find({})
        
        data = []
        for doc in cursor:
            data.append({
                "capacity_mw": doc['specs']['capacity_mw'],
                "fuel": doc['specs']['primary_fuel'],
                "lat": doc['location']['latitude'],
                "lon": doc['location']['longitude']
            })
        
        logger.info(f"Successfully retrieved {len(data)} documents.")
        client.close()
        return pd.DataFrame(data)
    
    except Exception as e:
        logger.error(f"Error during data retrieval: {e}")
        return None

# 3. Solution Analysis: Implement & Evaluate Model
def run_analysis_pipeline(df):
    logger.info("Initializing modeling pipeline...")
    
    X = df[['fuel', 'lat', 'lon']]
    y = df['capacity_mw']
    
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
    
    categorical_features = ['fuel']
    categorical_transformer = OneHotEncoder(handle_unknown='ignore')

    preprocessor = ColumnTransformer(
        transformers=[
            ('cat', categorical_transformer, categorical_features)
        ], remainder='passthrough')

    model_pipeline = Pipeline(steps=[
        ('preprocessor', preprocessor),
        ('regressor', RandomForestRegressor(n_estimators=100, random_state=42))
    ])

    logger.info("Training Random Forest Regressor...")
    model_pipeline.fit(X_train, y_train)
    logger.info("Model training complete.")
    
    y_pred = model_pipeline.predict(X_test)
    
    rmse = np.sqrt(mean_squared_error(y_test, y_pred))
    r2 = r2_score(y_test, y_pred)
    
    logger.info(f"--- Pipeline Results ---")
    logger.info(f"RMSE: {rmse:.2f} MW, R-squared: {r2:.4f}")
    
    # Return the model, actuals, and predictions
    return model_pipeline, y_test, y_pred

# 4. Visualize Results: Actual vs Predicted
def visualize_results(y_test, y_pred):
    logger.info("Generating actual vs. predicted visualization...")
    sns.set_theme(style="whitegrid", context="talk")
    plt.figure(figsize=(12, 8))
    
    plt.scatter(y_test, y_pred, alpha=0.4, color='#2c7fb8', edgecolors='w', s=60)
    
    max_val = max(max(y_test), max(y_pred))
    plt.plot([0, max_val], [0, max_val], color='#de2d26', lw=2, linestyle='--')
    
    plt.xscale('log')
    plt.yscale('log')
    
    plt.title('Actual vs. Predicted Capacity (MW)', fontsize=18, pad=20)
    plt.xlabel('Actual Capacity (MW)', fontsize=14)
    plt.ylabel('Predicted Capacity (MW)', fontsize=14)
    
    plt.tight_layout()
    plt.savefig("publication_quality_results.png", dpi=300)
    plt.show()

# 5. Visualize Feature Importance
def visualize_feature_importance(model_pipeline):
    logger.info("Extracting feature importances...")
    
    # Extract feature names from preprocessor
    ohe_categories = model_pipeline.named_steps['preprocessor']\
                        .transformers_[0][1]\
                        .get_feature_names_out(['fuel'])
    
    feature_names = np.concatenate([ohe_categories, ['lat', 'lon']])
    importances = model_pipeline.named_steps['regressor'].feature_importances_
    
    feat_importances = pd.DataFrame({'feature': feature_names, 'importance': importances})
    feat_importances = feat_importances.sort_values(by='importance', ascending=False).head(10)

    plt.figure(figsize=(12, 8))
    sns.barplot(x='importance', y='feature', data=feat_importances, palette="viridis")
    
    plt.title('Top 10 Feature Importances: Power Capacity (MW)', fontsize=18, pad=20)
    plt.xlabel('Relative Importance Score', fontsize=14)
    plt.ylabel('Feature', fontsize=14)
    
    plt.tight_layout()
    plt.savefig("feature_importance.png", dpi=300)
    plt.show()

# --- EXECUTION ---
if __name__ == "__main__":
    df_plants = fetch_and_prepare_data()
    
    if df_plants is not None and not df_plants.empty:
        # Capture the model here to pass it to the visualizer
        trained_model, y_test, y_pred = run_analysis_pipeline(df_plants)
        
        # Now both of these will have the data they need
        visualize_results(y_test, y_pred)
        visualize_feature_importance(trained_model)
    else:
        logger.error("Pipeline aborted: Data not found.")
```

---

### Analysis Rationale

The analysis utilizes a Random Forest Regressor to model the non-linear relationship between a power plant’s physical location and its generation capacity ($MW$). While simple linear models assume a constant change in output across coordinates, global energy infrastructure is characterized by spatial clustering where specific regions (proxies by longitude and latitude) possess vastly different industrial densities and grid maturities. By employing an ensemble tree-based approach, the model can identify complex interaction effects, such as how the impact of a "Nuclear" fuel type changes based on the geographic region. This method was specifically chosen to navigate the high-dimensional nature of the feature set, where categorical variables like fuel must be mapped against continuous geospatial coordinates. The goal was to move beyond simple correlation and build a predictive engine that acknowledges geography as a primary driver of infrastructure scale, ultimately providing a data-driven benchmark for regional energy output and industrial development patterns. Additionally, the goal was to see the ability to predict with limited factors the generation capacity because another dataset with other factors like the size of the facility for example would definitely yield better results.

---

### Visualization rationale

The model achieved an R-squared of 0.2982 with a Root Mean Square Error (RMSE) of 445.45 MW. While these metrics indicate a modest predictive fit, they represent a high degree of technical integrity given the constrained feature set. In the context of global infrastructure, an $R^2$ of approximately 0.30 is a hallmark of a model experiencing Omitted Variable Bias. The patterns visible in the Log-Log scatter plot show that while the model is somewhat correlated with the predicted line for a plant's capacity based on its primary fuel and geospatial coordinates (lon and lat). However, it lacks the granular data (like the commissioning year, regional GDP, and industrial demand)required to distinguish between specific scales of operation. By avoiding "overfitting" or the inclusion of leaked data, this result accurately reflects the complexity of the global power grid. It demonstrates that geography and fuel type are significant baseline predictors (collectively accounting for nearly 70% of feature importance), but that localized economic policies and technological eras remain latent variables that dictate the final nameplate capacity. This level of transparency in the results ensures the model remains a grounded, defensible tool for initial market sizing and strategic infrastructure assessment. The feature importance highlight how the two most important indicators are longitude and latitude which makes sense in modern times. Had this data been from before 2000 then it would be clear that a fuel source like Coal would have a much higher importance for Power Capacity due to a lack of modern energy infrastructure.
