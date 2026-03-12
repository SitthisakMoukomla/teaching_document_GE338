# 🔥 Wildfire Risk Prediction in Chiang Mai
### Spatial Statistics & Geographic Modeling with Google Earth Engine

**Course:** GE 234 Basic Programming for Geographers  
**Instructor:** ผศ.ดร.สิทธิศักดิ์ หมูคำหล้า  
**Department of Geography, Faculty of Liberal Arts, Thammasat University**

---

## 📋 Overview

Jupyter Notebook นี้สอน workflow ตั้งแต่ **Spatial Statistics** ไปจนถึง **Machine Learning Classification** สำหรับทำนายความเสี่ยงไฟป่าในจังหวัดเชียงใหม่ โดยใช้ Google Earth Engine (GEE) เป็น platform หลัก

**พื้นที่ศึกษา:** จังหวัดเชียงใหม่ (AOI: 98.5–99.5°E, 18.5–19.5°N)  
**ช่วงเวลา:** มกราคม–เมษายน 2567 (ฤดูแล้ง/ฤดูไฟป่า)  
**Resolution:** 30 เมตร (Landsat 9)

---

## 🗂️ Notebook Structure

```
Part 1: Spatial Statistics Fundamentals
├── 1.1  Tobler's First Law of Geography
├── 1.2  Moran's I — Global Spatial Autocorrelation
│        ├── Moran Scatter Plot
│        ├── LISA Cluster Map (Local Moran's I) ← 🗺️ แผนที่
│        └── Interactive Fire Risk Map (Folium) ← 🗺️ แผนที่
└── 1.3  Cloud Problem & Gap Filling
         ├── focal_mean() in GEE
         └── IDW Interpolation

Part 2: Geographic Modeling with GEE
├── 2.0  Study Area — AOI Context Map ← 🗺️ แผนที่
├── 2.1  Layer Stacking (NDVI, LST, Slope, Rainfall)
├── 2.2  Random Forest Classification
│        ├── Training Data Preparation
│        ├── Train/Test Split (70:30)
│        └── Feature Importance
├── 2.3  Accuracy Assessment
│        ├── Confusion Matrix
│        ├── Overall Accuracy & Kappa
│        └── Producer's & User's Accuracy (per class)
└── 2.4  Model Comparison: Random Forest vs CART
```

---

## 🗺️ Maps in This Notebook

| Cell | Map | Library | รายละเอียด |
|:---:|:---|:---:|:---|
| 1.2c | LISA Cluster Map | matplotlib | Hot spot / Cold spot / Outlier |
| 1.2e | Interactive Fire Risk | folium | คลิก popup, Heat map layer |
| 2.0 | AOI Context Map | folium | Satellite basemap, key locations |
| 1.3a | NDVI Gap Filling | geemap | Toggle 3 layers เปรียบเทียบ |
| 2.1b | 4-Layer Stack | geemap | NDVI, LST, Slope, Rainfall |
| 2.2d | Wildfire Risk Map | geemap | Classification result |
| 2.4b | RF vs CART | geemap | Side-by-side comparison |

---

## 📦 Input Data

| ข้อมูล | Source | GEE Asset ID | ใช้ทำ |
|:---|:---:|:---|:---|
| Landsat 9 L2 | USGS | `LANDSAT/LC09/C02/T1_L2` | NDVI, LST |
| SRTM DEM | USGS/NASA | `USGS/SRTMGL1_003` | Slope |
| CHIRPS Daily | UCSB | `UCSB-CHG/CHIRPS/DAILY` | Rainfall |

> **หมายเหตุ:** Training points ใน notebook นี้เป็นข้อมูลจำลอง (simulated) เพื่อการสอน  
> ในงานวิจัยจริงควรใช้ FIRMS/VIIRS hotspot data + field survey

---

## 🛠️ Requirements

### Python Libraries

```bash
pip install geemap earthengine-api pysal libpysal esda splot folium branca
```

| Library | Version | ใช้ทำ |
|:---|:---:|:---|
| `earthengine-api` | ≥ 0.1.370 | Google Earth Engine Python API |
| `geemap` | ≥ 0.30 | Interactive GEE maps ใน Jupyter |
| `pysal` / `libpysal` | ≥ 4.7 | Spatial weights matrix |
| `esda` | ≥ 2.5 | Moran's I, LISA |
| `splot` | ≥ 1.1 | Visualization สำหรับ spatial stats |
| `folium` | ≥ 0.15 | Interactive map บน basemap จริง |
| `branca` | ≥ 0.7 | Color scales สำหรับ folium |
| `geopandas` | ≥ 0.13 | GeoDataFrame |
| `scipy` | ≥ 1.11 | Gaussian filter |

### Google Earth Engine Account

1. สมัคร GEE account ที่ [earthengine.google.com](https://earthengine.google.com)
2. สร้าง GEE Project ที่ [code.earthengine.google.com](https://code.earthengine.google.com)
3. แก้ไข Project ID ใน Cell 0:
   ```python
   ee.Initialize(project='YOUR-PROJECT-ID')
   ```

---

## 🚀 Getting Started

### Option A: Google Colab (แนะนำ)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

1. Upload ไฟล์ `.ipynb` ไปยัง Google Drive
2. เปิดด้วย Google Colab
3. Run Cell 0 เพื่อ install libraries และ authenticate GEE
4. แก้ไข Project ID แล้ว Run ทีละ cell

### Option B: Local Jupyter

```bash
# Clone repository
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
cd YOUR_REPO

# Install dependencies
pip install -r requirements.txt

# Launch Jupyter
jupyter notebook Spatial_Stats_GEE_WildfireRisk_maps.ipynb
```

---

## 📊 Key Concepts

### Part 1: Spatial Statistics

**Tobler's First Law**
> "Everything is related to everything else, but near things are more related than distant things."

เป็นพื้นฐานที่รองรับทุก technique ใน notebook นี้ — ตั้งแต่ Gap Filling ไปจนถึง Random Forest

**Global Moran's I**  
วัดว่า fire risk มี spatial clustering ทั้งพื้นที่หรือไม่ (I > 0 = clustered, I ≈ 0 = random)

**LISA (Local Moran's I)**  
ระบุตำแหน่งที่แน่นอนของ Hot Spot (HH), Cold Spot (LL), และ Spatial Outlier (HL/LH) ที่มีนัยสำคัญทางสถิติ (p < 0.05)

### Part 2: Geographic Modeling

**Layer Stacking**  
รวม 4 ปัจจัยความเสี่ยงเป็น multi-band image: NDVI + LST + Slope + Rainfall

**Random Forest**  
Ensemble ของ Decision Tree 50 ต้น — แต่ละต้นเห็นข้อมูล subset แบบ random แล้ว vote รวมกัน

**Accuracy Assessment**  
ใช้ Confusion Matrix, Overall Accuracy, Kappa Coefficient, Producer's Accuracy, User's Accuracy

---

## 📈 Expected Results

| Metric | ค่าที่คาดหวัง | เกณฑ์คุณภาพ |
|:---|:---:|:---:|
| Overall Accuracy | > 80% | ≥ 75% = acceptable |
| Kappa Coefficient | > 0.60 | > 0.60 = substantial |
| High Risk Recall | > 70% | สำคัญ — miss hotspot = อันตราย |
| RF vs CART | RF ดีกว่า | ensemble effect |

---

## 📝 Exercises

1. **Moran's I** — เปลี่ยน `k` ใน KNN weights (4, 8, 12) แล้วสังเกตผล
2. **Gap Filling** — เปลี่ยน `radius` ของ `focal_mean()` (1, 3, 5, 10 pixels)
3. **Layer Stacking** — เพิ่ม NDWI หรือ EVI เป็น band ที่ 5 แล้ว retrain
4. **Model Tuning** — เปลี่ยน `numberOfTrees` (10, 50, 100, 200)
5. **Model Comparison** — ลอง `smileGradientTreeBoost` แทน Random Forest

---

## 🔗 References

- Tobler, W. R. (1970). A Computer Movie Simulating Urban Growth in the Detroit Region. *Economic Geography*, 46, 234–240.
- Anselin, L. (1995). Local Indicators of Spatial Association—LISA. *Geographical Analysis*, 27(2), 93–115.
- Breiman, L. (2001). Random Forests. *Machine Learning*, 45(1), 5–32.
- Chave, J. et al. (2014). Improved allometric models to estimate the aboveground biomass of tropical trees. *Global Change Biology*, 20(10), 3177–3190.
- Google Earth Engine: [developers.google.com/earth-engine](https://developers.google.com/earth-engine)
- GeoPandas: [geopandas.org](https://geopandas.org)
- PySAL: [pysal.org](https://pysal.org)

---

## 📁 Repository Structure

```
.
├── Spatial_Stats_GEE_WildfireRisk_maps.ipynb   # Main notebook
├── README.md                                    # This file
└── requirements.txt                             # Python dependencies (optional)
```

---

## 📄 License

เนื้อหานี้จัดทำเพื่อการเรียนการสอน ภาควิชาภูมิศาสตร์ มหาวิทยาลัยธรรมศาสตร์  
สามารถนำไปใช้เพื่อการศึกษาได้โดยอ้างอิงแหล่งที่มา

---

*Last updated: มีนาคม 2568 | GE 234, Thammasat University*
