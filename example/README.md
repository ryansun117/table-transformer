## Table Transformer for table extraction overview

Table Transformer is a DETR (DEtection TRansformer) model, which consists of a convolutional backbone (ResNet-50 or ResNet-101) followed by an encoder-decoder Transformer, and can be trained end-to-end to perform object detection. 

The model generalizes table extraction into two subtasks: 

1. Table Detection (TD), which locates the table and returns table bounding box coordinates.
2. Table Structure Recognition (TSR), which recognizes a table’s rows, columns, and cells, and Functional Analysis (FA), which recognizes a table’s keys and values.

Reference: https://github.com/NielsRogge/Transformers-Tutorials/blob/master/Table%20Transformer/Using_Table_Transformer_for_table_detection_and_table_structure_recognition.ipynb

## Table Transformer (DETR model) table detection visualization

Link: 

## How can we use it? 

The DETR model detects where tables, table rows, and columns etc. are in an image. The inference code built on it needs text extraction (from OCR or directly from PDF) as a separate input in order to include text in its HTML or CSV output. Therefore, incorporating Table Transformer or similar models into COSMOS (potentially) is a two step process: 

1. Replace the existing COSMOS process that detects tables with the model's (1) Table Detection task to return table coordinates. This may require a detailed comparison between the two. The PDF files need to be converted into images.

2. Crop the image based on the bounding box coordinates provided by Table Transformer, then, use a seperate tool or model to extract text from the detected table. Such tools include Camelot and Pdfplumber ([ref. huggingface](https://huggingface.co/microsoft/table-transformer-detection/discussions/3#63f7c541ae70dee48030e860)). Such models include optical character recognition models like TrOCR ([ref. issue](https://github.com/microsoft/table-transformer/issues/68#issuecomment-1240917664)), DONUT ([ref. huggingface](https://huggingface.co/microsoft/table-transformer-detection/discussions/3#63f872a54a7daa003c9c440e)), Layout Parser ([example](https://github.com/Layout-Parser/layout-parser/blob/main/examples/OCR%20Tables%20and%20Parse%20the%20Output.ipynb)), and Tesseract (might not work [ref.](https://github.com/microsoft/table-transformer/issues/68#issuecomment-1241876689)).

Another idea: Use the model's (2) TSR + FA task to identify the cell layouts of a table and use them to customize Pdfplumber's table extraction [settings](https://github.com/jsvine/pdfplumber#table-extraction-settings).

Overall, the conclusion is that currently no single tool can perform PDF-to-CSV table extraction well. The best bet to try something new is using a table detection model + an optical character recognition models.
