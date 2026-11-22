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

dot -Tpng docs/diagrams/use_cases.dot -o docs/diagrams/use_cases.png
dot -Tpng docs/diagrams/activite_commande.dot -o docs/diagrams/activite_commande.png
dot -Tpng docs/diagrams/activite_couverture.dot -o docs/diagrams/activite_couverture.png
dot -Tpng docs/diagrams/sequence_assign.dot -o docs/diagrams/sequence_assign.png
dot -Tpng docs/diagrams/sequence_create_ue.dot -o docs/diagrams/sequence_create_ue.png
dot -Tpng docs/diagrams/sequence_trace_graph.dot -o docs/diagrams/sequence_trace_graph.png

dot -Tpng docs/conception/classes_it1.dot -o docs/conception/classes_it1.png
