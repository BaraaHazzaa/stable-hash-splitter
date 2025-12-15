# Publishing and GitHub instructions

1. Create a GitHub repository (replace placeholders):

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/BaraaHazzaa/stable-hash-splitter.git
git push -u origin main
```

2. Create a Test PyPI account and a real PyPI account.

3. Build the package and upload to Test PyPI (recommended):

```bash
pip install build twine
python -m build
python -m twine upload --repository testpypi dist/*
```

4. Install from Test PyPI to verify:

```bash
pip install --index-url https://test.pypi.org/simple/ stable-hash-splitter
```

5. Upload to real PyPI:

```bash
python -m twine upload dist/*
```

Notes:
- Use GitHub Actions for CI (tests are included in `.github/workflows/ci.yml`).
- Add a PyPI API token as a GitHub secret (`PYPI_API_TOKEN`) if you want to automate releases.
