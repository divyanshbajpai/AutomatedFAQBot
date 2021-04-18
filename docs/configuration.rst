Configuration
================


``NeuralQA`` provides an interface to specify properties of each module (ui, retriever, reader, expander) via a `yaml configuration <neuralqa/config_default.yaml>`_ file. When you launch the ui, you can specify the path to your config file `--config-path`. If this is not provided, NeuralQA will search for a config.yaml in the current folder or create a [default copy](neuralqa/config_default.yaml)) in the current folder. Sample configuration for the UI is shown below:


UI Configuration
**************************

The code snippet below shows how you can configure parts of the main user interface for ``NeuralQA``.

.. note::
    You will need to restart ``NeuralQA`` each time you make a change to config.yaml.
    You can show/hide sections of the UI e.g. show/hide retrieved passages, show only top answer or all answers, show or hide the advanced options view etc. You can also change the default title and description of the page.


.. code-block:: yaml

    ui:
    header:
        appname: NeuralQA
        appdescription: Question Answering on Large Datasets
    queryview:
        intro:
        title: "NeuralQA: Question Answering on Large Datasets"
        subtitle: "NeuralQA is an interactive tool for question answering (passage retrieval + document reading). You can manually provide a passage or select a search index from (e.g. case.law ) dataset under the QA configuration settings below. To begin, type in a question query below."
        disclaimer: " .. "
        views:
        intro: True
        advanced: True # if false, default retriever/reader settings will be used.
        samples: True # show/hide sample question answer pairs
        passages: True # show/hide passages which are retrieved
        explanations: True # show/hide explanations button
        allanswers: True # show all answers or just the best answer (based on probability score)
        expander: False # show or hide the expander dropdown.
        options:
        stride:
            title: Token Stride
            selected: 0
            options:
            - name: 0
                value: 0
            - name: 50
                value: 50
            - name: 100
                value: 100
            - name: 200
                value: 200
        maxdocuments:
            title: Max Documents
            selected: 5
            options:
            - name: 5
                value: 5
            - name: 10
                value: 10
            - name: 15
                value: 15
        highlightspan:
            title: Highlight Span
            selected: 250
            options:
            - name: 150
                value: 150
            - name: 250
                value: 250
            - name: 350
                value: 350
            - name: 450
                value: 450
            - name: 650
                value: 650
        samples:
 


Reader Configuration
**************************

You can configure the reader models that are available for use with the ``NeuralQA`` api and web interface. Because ``NeuralQA`` uses the HuggingFace api, reader models can be specified using either the path to a hosted HuggingFace model or the path to a local folder on disk contained a trained HuggingFace model.

.. code-block:: yaml

    reader:
    title: Reader
    selected: twmkn9/distilbert-base-uncased-squad2  #default selected reader on startup. Should correspond to the reader value
    options:
        - name: DistilBERT SQUAD2
        value: twmkn9/distilbert-base-uncased-squad2
        type: distilbert
        - name: BERT SQUAD2
        value: deepset/bert-base-cased-squad2
        type: bert
        - name: Medical BERT SQUAD2
        value: /Users/user/Downloads/meddistilbert  # example path to a local model on disk  
        type: bert





Memory Requirements
**************************

To enable fast user interaction, ``NeuralQA`` loads models weights that are specified in `config.yaml` into memory when the application is launched. For example, if 3 Bert base reader models (~425MB each) specified in  `config.yaml`, each of these will be loaded into memory. Similarly all specified query expansion models will also be loaded into memory. Plan to account for these when using ``NeuralQA``.