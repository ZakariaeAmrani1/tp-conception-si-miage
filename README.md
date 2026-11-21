# tp-conception-si-miage

python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt

####python -m pip install --upgrade pip

###pip freeze > requirements.txt

mkdir src, tests, docs, data
ni src\_\_init**.py, tests\_\_init**.py
ni docs\roles.md, docs\journal_iterations.md, docs\suivi_decisions.md, docs\indicateurs.md
ni README.md, .flake8, .gitignore

python -m unittest discover -s tests -t .
flake8 src tests
dot -V
