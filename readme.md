# 🧠 ReWear ML Module — AI-Powered Clothing Auto-Tagging

This module enables **automatic clothing type detection** when users upload an image while listing a new item on the ReWear platform.

---

## 🎯 Objective

To improve user experience and streamline the item listing process by integrating a **lightweight, client-side machine learning model** that:

- Predicts the **clothing category/type** (e.g., T-shirt, Dress, Kurta)
- Auto-fills the relevant form field based on image input

---

## 🛠️ Tech Stack

- **Model Source**: [Teachable Machine by Google](https://teachablemachine.withgoogle.com/)
- **Inference Runtime**: TensorFlow.js
- **Deployment**: Runs directly in the browser (no server-side compute required)

---

## 🧪 Model Details

- **Type**: Image Classification
- **Training Tool**: Teachable Machine GUI
- **Classes**:
  - T-shirt
  - Kurta
  - Dress
  - Jeans
  - Shirt
  - Jacket
- **Training Data**: ~10 sample images per class (custom, small-scale for demo)

---

## ⚙️ How It Works

1. User uploads an image on the “Add New Item” form  
2. The model (loaded via TensorFlow.js) performs prediction  
3. The top class is automatically filled into the **Category** field of the form  

---

## 🔮 Future Work

- Improve model with more diverse and higher-quality data
- Expand tags to include:
  - **Material** (cotton, denim, wool)
  - **Occasion** (casual, formal)
  - **Condition** (new, lightly used)
- Integrate feedback loop to refine predictions

---

## 🙌 Credits

- Model trained using Google Teachable Machine  
- TensorFlow.js used for real-time browser inference

> Developed as part of the **ReWear Hackathon Submission** to demonstrate how AI can enhance sustainable fashion platforms.
