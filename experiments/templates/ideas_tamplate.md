# 💭 Experiment Ideas

> Purpose: Collect untested or future experiment ideas in one place for later prioritization.

---

## 🧠 Template for a Good Idea

### Title
Try Entity Embeddings for Categorical Variables

### Rationale
We have several high-cardinality categorical features (e.g. user_id, product_id) which might benefit from learning dense vector representations.

### Expected Benefit
May capture deeper patterns across users and products, improving model generalization.

### Reference / Inspiration
https://arxiv.org/abs/1604.06737

### Tools Needed
- PyTorch or TensorFlow
- Embedding layers
- Custom dataloader

### Risks / Considerations
- Might overfit if embedding dimensions are too large
- Training time will increase

### Priority
🌟 High — revisit after baseline LightGBM experiments

---

## 📌 Other Ideas

- Try stacking LightGBM + CatBoost
- Use PCA for feature reduction on numerical features
- Ensemble top 3 models with weighted voting
