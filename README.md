* Final kw mapping output: output/finance_kw_mapping_result.txt

Data:
* Finance md file: https://drive.google.com/drive/u/1/folders/1LhaMVbtpuCKSCRuiffxTn05pOIJJSIXU
* Finance document kw extraction: output/finance_kw_extraction.json
* Finance query kw extraction: output/finance_kw_extraction.json

Code:
* ocr_and_kw_extraction.ipynb: 
    * document preprocess: PDF2Image -> OCR -> use cahtgpt translate ocr result to md -> kw extraction
    * question preprocess: kw extraction
* document_retriever.ipynb: match question and document



Prompt:

* use cahtgpt translate ocr result to md 
```
prompt = (
    f"Convert the OCR results into Markdown format. "
    f"The input is a list of text extracted by the OCR model, containing both free text and tables from {property}-related documents. "
    f"Please transform this list into Markdown format, correcting any typos you find.\n"
    f"Output the result in JSON format, like this: {{'markdown': 'generate markdown content here'}}.\n"
    f"Input text: {text_list}\n"
)
```

* document/question kw extraction
```
keyword_extraction_prompt = (
    f"Please extract keywords from the input text, which may be in English or Traditional Chinese. "
    f"The input text could be a sentence or content in Markdown format. "
    f"Extract the keywords and classify them into the following categories: Person, Place, Company, Time, or {property} terminology."
    f"Category Descriptions:"
    f"Person: Names of individuals"
    f"Place: e.g., Taipei"
    f"Company: Names of companies in Taiwan, e.g., 長榮, 亞德客-KY, or 光寶科技股份有限公司"
    f"Time: e.g., 2022年第3季"
    f"{property} terminology: Specific terms, e.g., 要保人, 受益人, 綜合損益總額, 淨現金流出, 合併權益變動表, 資產總額, 合併資產總額, 營業利益"
    f"\nPlease output in JSON format, like this example: {{'Person': [], 'Place': [], 'Company': ['長榮'], 'Time': [], '{{property}} terminology': ['綜合損益總額']}}"
    f"The input text is {text}"
)
```