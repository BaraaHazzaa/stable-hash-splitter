# Stable Hash Splitter

[![PyPI version](https://badge.fury.io/py/stable-hash-splitter.svg)](https://pypi.org/project/stable-hash-splitter/)
[![Python versions](https://img.shields.io/pypi/pyversions/stable-hash-splitter)](https://pypi.org/project/stable-hash-splitter/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A **scikit-learn compatible splitter** for **deterministic, ID-based train/test splits** that prevent data leakage in machine learning workflows.

## 🔧 The Problem

When datasets grow or get updated, traditional random splits can cause **data leakage**: samples that were previously in your test set might end up in training during retraining, leading to overly optimistic and invalid model evaluations.

**StableHashSplit** solves this by assigning samples to train/test sets **deterministically** based on a hash of a stable identifier (e.g., user ID, transaction ID). Once assigned, a sample stays in the same set forever, ensuring reproducible and reliable evaluations across dataset versions.

## ✨ Key Features

- **🔒 Deterministic & Stable**: Same ID always maps to the same split
- **🤖 Scikit-Learn Compatible**: Works seamlessly with `GridSearchCV`, `cross_val_score`, and ML pipelines
- **📊 Flexible Inputs**: Supports pandas DataFrames, NumPy arrays, and array-like structures
- **⚙️ Customizable**: Choose your hash function and ID column
- **🚀 Simple API**: Minimal code changes needed

## 📦 Installation

```bash
pip install stable-hash-splitter
```

## 🚀 Quick Start

```python
import pandas as pd
from stable_hash_splitter import StableHashSplit

# Sample data with user IDs
data = pd.DataFrame({
    'user_id': [1001, 1002, 1003, 1004, 1005],
    'feature_1': [0.5, 0.3, 0.8, 0.1, 0.9],
    'feature_2': [10, 20, 30, 40, 50],
    'target': [1, 0, 1, 0, 1]
})

# Create stable splitter
splitter = StableHashSplit(test_size=0.2, id_column='user_id')

# Split your data
X_train, X_test, y_train, y_test = splitter.train_test_split(
    data[['user_id', 'feature_1', 'feature_2']],
    data['target']
)

print(f"Train size: {len(X_train)}, Test size: {len(X_test)}")
# Output: Train size: 4, Test size: 1
```

## 📚 Advanced Usage

### Using with GridSearchCV

```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import GridSearchCV

splitter = StableHashSplit(test_size=0.2, id_column='user_id')
model = RandomForestClassifier()

param_grid = {'n_estimators': [50, 100], 'max_depth': [5, 10]}
grid_search = GridSearchCV(model, param_grid, cv=splitter)
grid_search.fit(X, y)  # X must contain the 'user_id' column

print(f"Best params: {grid_search.best_params_}")
```

### Custom Hash Function

```python
import hashlib

def custom_hash(id_value):
    return int(hashlib.md5(str(id_value).encode()).hexdigest(), 16)

splitter = StableHashSplit(
    test_size=0.3,
    id_column='user_id',
    hash_func=custom_hash
)
```

## 📖 API Reference

### StableHashSplit

```python
StableHashSplit(test_size=0.2, id_column='id', hash_func=None, random_state=None)
```

**Parameters:**
- `test_size` (float): Fraction of samples for test set (0 < test_size < 1)
- `id_column` (str | int | None): Column name/index with stable IDs. Uses DataFrame index if None
- `hash_func` (callable): Function mapping ID to non-negative integer. Defaults to CRC32
- `random_state`: Ignored (for scikit-learn compatibility)

**Methods:**
- `split(X, y=None)`: Returns train/test indices
- `get_n_splits()`: Returns 1 (single split)
- `train_test_split(X, y)`: Convenience method for direct splitting

## 🤝 Contributing

We welcome contributions! Please:

1. Open an issue to discuss your idea
2. Fork the repository
3. Create a feature branch
4. Submit a pull request

For development setup, see `PUBLISH.md`.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Attribution

Inspired by ID-based splitting concepts from Aurélien Géron's "Hands-On Machine Learning with Scikit-Learn and PyTorch". This is an independent implementation.

## 📞 Support

- 📧 Email: baraa-hazaa00@hotmail.com
- 🐛 Issues: [GitHub Issues](https://github.com/BaraaHazzaa/stable-hash-splitter/issues)
- 📚 Documentation: This README and docstrings
