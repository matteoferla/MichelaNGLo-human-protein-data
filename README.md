## data

The whole dataset used by MichelaNGLo is large. For it you will have to contact Matteo or regenerate the data. This is only the human proteome.

See [Deployment notes for installation](https://github.com/matteoferla/MichelaNGLo-app/blob/master/git_docs/deploy.md).

Note that the [protein parsing module](https://github.com/matteoferla/MichelaNGLo-protein-module) is dependent on only the transpiler module, which is required for the structural analysis of a variant only &mdash;if unneeded alter `analyse/__init__.py` to not use `PyMOL_structure_analyser.py`.


## Dictionaries

In this repo are some JSON files intended for the `dictionary` subfolder of the protein-data folder and a tsv for the `reference` subfolder. The rest are for decompression into the `pickle/taxid9606` subfolder 
–taxon id 9606 is human.

## Usage

The files are gip compressed pickle files labelled by Uniprot accession.

To read a file:

    import os
    from michelanglo_protein import ProteinCore
    ProteinCore.settings.startup(data_folder='protein-data')
    p = ProteinCore().gload(file=os.path.join('gpickle','Q86V25.gpz'))

    from pprint import PrettyPrinter
    pprint = PrettyPrinter().pprint
    pprint(p.asdict())

Alternatively, importing `ProteinAnalyser`:

    from michelanglo_protein import ProteinAnalyser
    ProteinAnalyser.settings.startup(data_folder='protein-data')
    p = ProteinAnalyser().gload(file=os.path.join('gpickle','Q86V25.gpz'))
    p.mutation = Mutation('p.N127W')
    if p.check_mutation():
        p.predict_effect()
        p.analyse_structure()
        pprint(p.asdict()['structural']
    else:
        p.mutation_discrepancy()


Do note that the class attribute `.settings` is actually a singleton instance, so `ProteinAnalyser.settings` is the same as `ProteinCore.settings`. Whereas `settings.startup()` is compulsory, `settings.retrieve_references(ask=False, refresh=False)` is not and in the case of `ProteinAnalyser` only the csv of ELM classes will be needed within the data folder in references, but will complain only otherwise.



