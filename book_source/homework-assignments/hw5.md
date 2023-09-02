# <i class="fas fa-laptop fa-fw"></i> HW5 - Search Engine

## Overview
**Learning objective:** Implement specialized data types with Python classes for tf-idf information retrieval.  

## Files
* `search_engine.py` is the file representing the SearchEngine class as well as a command-line interface for using the SearchEngine.  
* `document.py` is the file representing a single document in the SearchEngine.  
* `hw5_test.py` is the file for you to put your own tests. The Run button executes this program.  
* `server.py` and `index.html` provide a web app for you to run your completed search engine. You do not need to look at or modify these files ever.  
* `cse163_utils.py` is a helper file that has code to help you test your code.    

```{admonition} Time Management
:class: warning
This assessment is very information-dense. Plan ahead by taking notes of what you need to do and where you will need to do it. It will probably help to get a global view of the entire assessment by reading **ALL** of the components before starting any coding—we expect students to spend at least 40 minutes reading, synthesizing, and planning before starting any coding.
```

```{admonition} Video on TF-IDF
:class: seealso
Watch the video of [Hunter Schafer](https://www.loom.com/share/118366e8ba1a427dad84870b869b660a?sid=d7353687-52cf-4ab3-a737-ed743bed0971).
```

## Context
A search engine is an algorithm that takes a query and retrieves the most relevant documents for that query. In order to identify the most relevant documents, our search engine will use term frequency–inverse document frequency (tf–idf), a text information statistic for determining the relevance of a term to each document from a corpus consisting of many documents.

The tf-idf statistic consists of two components: term frequency and inverse document frequency. Term frequency computes the number of times that a term appears in a document (such as a single Wikipedia page). If we were to use only the term frequency in determining the relevance of a term to each document, then our search result might not be helpful since most documents contain many common words such as “the” or “a”. In order to down-weight these common terms, the document frequency computes the number of times that a term appears across the corpus of all documents. The tf-idf statistic takes a term and a document and returns the term frequency divided by the document frequency.

In this assessment, we’ll implement two classes: a `Document` class to represent individual web pages and a `SearchEngine` class that aggregates the corpus of all `Document` objects. The `SearchEngine` builds on the `Document`, so be sure to fully complete the `Document` class before moving onto the `SearchEngine`.

## Topics
* Normalizing Tokens
* HTML Stripping
* Writing Tests
    * Test Methods
    * Do Not Change...
    * Normalize Paths
    * Sorting lists for comparison
* Submitting
* Important Tips
    * Commenting
    * Flake8
  
# Normalizing Tokens
In this project we need to remove punctuation so that all tokens that are semantically
identical can be recognized as "_exactly_" identical. For example:
```
'Hello!' == 'hello'
```
`normalize_token` is a helper method found in the `cse_utils.py` file.

Furthermore, in the handout it talks about
reading HTML files, only you'll discover that in this project there are no HTML files. Instead, I've already
stripped out all the `<tags>` and `<script>` for you. See below for a discussion on that.

The process of removing punctuation is called `normalization`. A quick an easy way to remove punctuation
is to use a [Regular Expression](https://docs.python.org/3/howto/regex.html) (more documentation [here](https://docs.python.org/3/library/re.html)). Regular
Expressions are used all over in Computer Science and you would benefit from learning
them. But, they can get complicated and they are out of scope for this class and project.
So, you get the code for free. [You're welcome!](https://youtu.be/79DijItQXMM)  

Just use the code that is found in `cse163_utils.py`.
```python
from cse163_utils import normalize_token
```

# HTML Stripping
In this ReplIt, you are provided with two folders with documents. When you run `main.py`, you should start off
by crawling the files in `test_corpus` or a folder you created with your own individual set of files. Eventually, 
_but not at the start_, you will
be able to quickly and effectively search `small_text`. Give it a shot before you submit your project!

The `small_text` folder is a subset
of wikipedia HTML articles with the HTML stripped away to reveal only the content. By stripping out
the HTML tags, there are some pros can cons.  

**Pros of Stripping:**  
* The developer can proceed without the distraction of how the code interfers with the solution.
* The indexing solution is not "confused" with needless code (the keywords and the non-English tag names).
* The size of the content is reduced.

**Cons of Stripping:** 
* The indexing solution actually works quite nicely on the HTML directly (with a few exceptions).
* The text has seemingly random spacing and an unusual occurance of the `^` character.
* Viewing the content is unappealing because all the nice formatting is lost. :-(

If you want to use the HTML version (small_wiki), you may download those files [here](https://drive.google.com/file/d/1PSCFeXbM5UCFU5pIkj5GbtXjgSDJXzoX/view?usp=share_link).  
> Note: The curious developer may be interested to know that small_text was created
> by using BeautifulSoup to strip the HTML files of their HTML. 

The name of the folder `small_text` may be a misnomer because it is 2.5 MB in size. That's
arguably large. However, there is a much larger corpus you can choose to use:  
* [Large Wikipedia HTML](https://drive.google.com/file/d/1A3K-PR_eJrMFvmCCP28y-haFcW8thrV_/view?usp=share_link)
* [Large Wikipedia Text](https://drive.google.com/file/d/15s5CkBwhW8cvQbUHKCjD_-jmeG5JsdhJ/view?usp=share_link)


Due to their size, these _Large Wikipedia_ folders should NOT be uploaded to Replit. Instead, you are encouraged
to download to your local computer and run HW4 locally. If your code is written efficiently, it will work if
given a little time. If your code is not written efficiently, patience will not suffice. :-)


# Writing Tests
You need to write your own tests. The tests should be simple enough that you can calculate
expected values by hand. Do not assume your code is correct and use those values in your tests. 

Your tests need to be commented so that I can understand them. Describe what you're testing.
Develop "enough" tests that you feel confident that your code is working correctly. Your tests
cannot be a copy of the Unit Tests provided. Although, you can examine those to see what kinds
of things you might test. I'm not claiming that the Unit Tests in the `test` directory is
easy to read. I'm not saying those test exemplify good style. They are tests that UW provided
and I used them "out of the box."

**IMPORTANT**: `hw4_test.py` needs to follow the Main-Pattern.

## Test Methods
Write many test methods. For example: do NOT write a single method for all of Document.
You should have at least one test method for each public method on the object. 
> For example, Document has: get_path, get_words, term_frequency.

And, there should be a test method for every "important" private method on the object.

> Example: SearchEngine has _calculate_idf

I've provided you with one fat Unit Test in Replit. It actually runs a bunch of tests 
in the `test` directory using the files in the `test_corpus` directory. This is not the
preferred Unit Test to have. It is provided to you as a Saftey Check only. You need
to write your own tests. The provided tests do not test every case!

You can get details of the Unit Test failures by updating .replit to run the file `run_tests.py`.

These tests are a gift to help you get extra verification that your code is working. 
More tests will be run during grading.

## Do Not Change...
Do not add, change or remove anything from the files in these directories.
Furthermore, do not add files to these directories:
* test
* test_corpus
* small_text

Do not change `cse163_utils.py`. Do NOT add any method to this file either!

## Normalize Paths
Due to how `os.join.path` works across operating systems, search results
need to be _normalized_. Use the following pattern to assure that your
expected paths equal your actual paths no matter where your tests run:

```python
def test_search():
    '''
    This method is an Example Test showing how to normalize paths.
    Do not include this test as one of your own!
    '''
    engine = SearchEngine('directory')
    expected = ['directory/doc1.txt', 'directory/doc2.txt']
    actual = engine.search('my query')
    # Please normalize the paths!!
    expected = normalize_paths(expected)
    actual = normalize_paths(actual)
    assert_equals(expected, actual)
```

## Submit 
Be sure to turn your files into `AutoGrader`. 
Turn in **ONLY** these python files:  
- document.py
- hw4_test.py
- search_engine.py

Please do **NOT** submit the `tox.ini` file, nor any of the other python files.  

You will want to submit all subdirectories of test files that you created. Yes, you'll want to
create test files in subdirectories. To submit your subdirectory to AutoGrader, you'll need
to do some extra work. Sorry! 

Zip your test subdirectories, but ONLY your test subdirectories. If you have more than one test
subdirectory, you can zip them both in the same zip file if you want, or you may keep them
separate and submit two zip files. Do **NOT** zip up any other files. Zip up only the test subdirectories.  

Here are some images to help you create a zip file for this project (on Windows).  
`TODO: ![ZipExample](assets/make_zip.jpg)`  
You'll see that the two test directories are selected. Then, we `right-click` on the selection
and choose `7-Zip -> Add to "HW4.zip"`. Of course, not all computers are the same, so, here is
another option for you.  
`TODO: ![ZipExample](assets/make_zip2.jpg)`  
Again, the two test directories are selected. Then, we `right-click` on the selection
and choose `Send to -> Compressed (zipped) folder"`.  

Either way, you'll submit a zip file of your test directories. Note that this is a new feature
in AutoGrader. I apologize for any bugs or hardships this may cause you (and especially me).

> IMPORTANT: You may **NOT** submit your code, or any file in the "root" directory in a `.zip`
file. Submit all other files one-by-one, normally.

Please do **NOT** submit the files in the following directories:  
* small_text  
* test  
* test_corpus  

## Important Tips
### Testing Output
Update the `.replit` file to run `run_tests.py` so that you can get more details
on the failures you're getting. Note that if you fail one of these Unit Tests,
then **your** Unit Tests are not thorough enough. Update them!!

### Performance
Performance is an important part of this project. You should implement your
project in ways that limits the processing. Don't **RE**process anything.

You should cache values/data in the Document and SearchEngine classes to avoid
expensive recalculations. Of course, a `search` will still require you to gather
TF-IDF values across all the documents that contain any term in the Search Term.  

When you test to see if something contains something, be sure this is a fast
operation. For example, the following is slow:
```python
# our example needs a long list
my_list = [ n**2 for n in range(200000)]
x = -1
if x in my_list:
    print('Well that "in" operation was slow!')
```
You should manually try out your project with the `small_text`. Do this by running `main.py`.
If your implementation fails to process this
directory in a reasonable amount of time, you have a flaw. You will not pass
the project if you cannot successfully do a search on the `small_text` directory.


### Normalize
The Unit Tests in Replit require that you `normalize` your terms in the Document 
method `term_frequency`. 

### Test Document Thoroughly
* Consider edge cases. Can you think of any?  
* Test term_frequency on at least two different words and documents where you
can easily calculate the values manually.  
* Search with UnNormalized! terms.

### Sorting Lists for comparison
When you `assert_equals` on two lists, the assert requires that the
lists be in the same order. **IF** you don't care about the order of the lists,
one way to verify that they are identical is to first
sort the lists.
```python
    # This test doesn't care about the order of the items.
    # This could be true for Document::get_words
    assert_equals(sorted(list1), sorted(list2))
```

## Commenting
You need to thoroughly comment `document.py` and `search_engine.py`.  

Every function needs a doc-string comment. 

Every class needs a doc-string.

You should add _some_ comments to `hw4_test.py`.

## Flake8
This project has added a `tox.ini` file which adds some configuration to the project. 
It turns off some rules and excludes `hw4_test.py` from `flake8`. It should make it a lot 
easier to pass the flake8 tests.  My intention is to have the same configuration while grading. 
You should not exclude more rules because I won't be turning off any othe rules in AutoGrader.

I will still require that you
demonstrate good style in `hw4_test.py`. My eye is pretty good at catching
bad style such as really long lines.

In other words, you 100% need to have flake8 pass as configured here!

Be careful of really long lines. If you have really long lines, I'll catch that with my
eye and you will be deducted points.