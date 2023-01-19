:original_name: dws_06_0092.html

.. _dws_06_0092:

Parsing Documents
=================

GaussDB(DWS) provides function **to_tsvector** for converting a document to the **tsvector** data type.

::

   to_tsvector([ config regconfig, ] document text) returns tsvector

**to_tsvector** parses a textual document into tokens, reduces the tokens to lexemes, and returns a **tsvector**, which lists the lexemes together with their positions in the document. The document is processed according to the specified or default text search configuration. Here is a simple example:

::

   SELECT to_tsvector('english', 'a fat  cat sat on a mat - it ate a fat rats');
                     to_tsvector
   -----------------------------------------------------
    'ate':9 'cat':3 'fat':2,11 'mat':7 'rat':12 'sat':4

In the preceding example we see that the resulting **tsvector** does not contain the words **a**, **on**, or **it**, the word **rats** became **rat**, and the punctuation sign (-) was ignored.

The **to_tsvector** function internally calls a parser which breaks the document text into tokens and assigns a type to each token. For each token, a list of dictionaries is consulted. where the list can vary depending on the token type. The first dictionary that recognizes the token emits one or more normalized lexemes to represent the token. For example:

-  **rats** became **rat** because one of the dictionaries recognized that the word **rats** is a plural form of **rat**.
-  Some words are recognized as stop words (see :ref:`Stop Words <dws_06_0104>`), which causes them to be ignored since they occur too frequently to be useful in searching. In our example these are **a**, **on**, and **it**.
-  If no dictionary in the list recognizes the token then it is also ignored. In this example that happened to the punctuation sign (-) because there are in fact no dictionaries assigned for its token type (**Space symbols**), meaning space tokens will never be indexed.

The choices of parser, dictionaries and which types of tokens to index are determined by the selected text search configuration. It is possible to have many different configurations in the same database, and predefined configurations are available for various languages. In our example we used the default configuration **english** for the English language.

The function **setweight** can be used to label the entries of a **tsvector** with a given weight, where a weight is one of the letters **A**, **B**, **C**, or **D**. This is typically used to mark entries coming from different parts of a document, such as title versus body. Later, this information can be used for ranking of search results.

Because **to_tsvector(NULL)** will return **NULL**, you are advised to use **coalesce** whenever a column might be **NULL**. Here is the recommended method for creating a **tsvector** from a structured document:

::

   CREATE TABLE tsearch.tt (id int, title text, keyword text, abstract text, body text, ti tsvector);

   INSERT INTO tsearch.tt(id, title, keyword, abstract, body) VALUES (1, 'book', 'literature', 'Ancient poetry','Tang poem Song jambic verse');

   UPDATE tsearch.tt SET ti =
       setweight(to_tsvector(coalesce(title,'')), 'A')    ||
       setweight(to_tsvector(coalesce(keyword,'')), 'B')  ||
       setweight(to_tsvector(coalesce(abstract,'')), 'C') ||
       setweight(to_tsvector(coalesce(body,'')), 'D');
   DROP TABLE tsearch.tt;

Here we have used **setweight** to label the source of each lexeme in the finished **tsvector**, and then merged the labeled **tsvector** values using the tsvector concatenation operator **\|\|**. For details about these operations, see :ref:`Manipulating tsvector <dws_06_0097>`.
