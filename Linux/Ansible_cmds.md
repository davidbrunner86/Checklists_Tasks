# Anlegen einer Collection mit Namespace
# idealerweise (!) in einer standardmäßigen Überstruktur  

mkdir -p collections/ansible_collections  
cd collections/ansible_collections/  

# anlegen der coll  
ansible-galaxy collection init MyNamespace.MyCollection  

# wechseln in unterordner und erstellen einer Rolle  
cd MyNamespace.MyCollection/roles  
ansible-galaxy role init MyRole  

# Die eigentliche Rolle ist dann unter /tasks/main.yml
# Ein Playbook kann die Rolle dann über MyNamespace.MyCollection.MyRole initiieren. Die Datei kann dabei zb. im verzeichnis ollections/ansible_collections liegen  
