# Language:101meta
A language for associating metadata with files in a file system.


## Summary [[1]](http://101companies.org/wiki/Language:101meta)
[Language:101meta](#language:101meta) is a rule-based language for connecting metadata with files and fragments
thereof. The constraints of a rule say which files to match, e.g., in terms of constraining the actual 
filename. Metadata consists of key-value pairs. In the [101project](http://101companies.org/wiki/@project), metadata
is concerned with usage of [languages](http://101companies.org/wiki/Software_language) and 
[technologies](http://101companies.org/wiki/@technology), with claims about the implementation of features, with 
tagging for terms of the [101companies:Vocabulary](https://github.com/101companies/101dev) and general concepts, as 
available on the [101wiki](http://101companies.org/wiki/@project). Conceptually, the language is not tied to the 
[101project](http://101companies.org/wiki/@project). The metadata is directly used for the exploration of the 
[101repo](https://github.com/101companies/101repo), as supposed by the 
[101companies:Explorer](http://101companies.org/resources?format=html&wikititle=@project). The official syntax of 
[Language:101meta](#Language:101meta) is [JSON-based](http://json.org/) with arbitrary JSON expressions for metadata
values. The [Language:101meta](#Language:101meta) is primarily meant to facilitate the representation of rules in a 
form that is directly useful for automated processing; usability of the notation for the end user is also a concern,
but a secondary one in the view of extra tool support helping with the use of the mechanism. For instance, the 
[101companies:Explorer](http://101companies.org/resources?format=html&wikititle=@project) provides support for 
authoring rules in an interactive manner so that the notation does not need to be manipulated directly.

The 101worker has a specific python library that handles the language. It will be called during the execution of the worker by multiple modules. You can fine the libarary in the 101worker repo (https://github.com/101companies/101worker/tree/master/libraries/meta101)


## Constraints 

There are two kinds of constrains in the current design of the [101meta language](#Language:101meta). One constraint
could be an aggregated metadata value. This constraint can just be used though, when it is safe to say that at the
time the rules will be executed the complete metadata values are already known. Therefore the modules have to let
the [101worker](https://github.com/101companies/101worker) know, through the metadata description, what metadatas
they have a constraint to or are retrieving during the execution. That can be compared to a pre and post condition
see more next paragraph.
Besides those constraints there are also the follow specific ones. These kinds of constraints can be used whenever
you want to. Hence they are independent from the execution time:

* **filename:** the name of a file to be matched. The name can also be specified by a pattern.
* **basename:** the basename of a file to be matched. The name can also be specified by a pattern. As usual, a
basename is a filename without any directory part.
* **suffix:** the suffix of a file to be matched. This is essentially a shorthand for a pattern to constrain only
the suffix (typically, the extension) of a filename.
* **dirname:** the name of the directory of a file to be matched. The name can also be specified by a pattern. The file must be contained in the specified directory or a subdirectory thereof.
* **content:** the content of a file to be matched based on regular expression to be applied to the text of the file.
* **fragment:** a fragment of a matched file to which to apply to metadata, subject to a suitable fragment description.
* **predicate:** the name of an executable to be applied to files for deciding on matching.


### In the 101project, the following forms of metadata are used:

* **language** for declaring an artifact as being an element of a language on the 101wiki.
* **partOf** for declaring an artifact as being a part of a technology on the 101wiki.
* **inputOf** for declaring an artifact as being consumed by a technology as input.
* **outputOf** for declaring an artifact as being produced by a technology as output.
* **dependsOn** for declaring an artifact as depending on a technology.
* **feature** for declaring a feature of the 101system as being implemented.
* **term** for association with a term of the 101companies:Vocabulary.
* **phrase** for association with a phrase built from the 101companies:Vocabulary.
* **concept** for association with a concept on the 101wiki.
* **nature** for association of file nature, e.g., binary for use by the 101companies:Explorer.
* **geshi** for association with a language code as used when rendering with Technology:GeSHi.
* **locator** for association with an executable to be used as fragment locator.
* **dominator** meta-metadata for sorting out priorities.
* **relevance** metadata for indicating the importance of a file.


## How the 101worker and Language:101meta come together

In that short chapter we want to discuss how the [101worker](https://github.com/101companies/101worker) and the [Language:101meta](#Language:101meta) fit together in the complete system. The modules with the `101` ending are somewhat related to the [Language:101meta](#Language:101meta). It is important to understand how they act together to gain an overall understanding.

First of all, all the 'rules', which are stored in the [101repo](https://github.com/101companies/101repo), are aggregated and summarized in one large file called [rules.json](http://data.101companies.org/dumps/rules.json). This job is performed by the [rules101meta](https://github.com/101companies/101worker/tree/master/modules/rules101meta) module.
After that module ran successfully the first basic rules are executed by the [matches101meta](https://github.com/101companies/101worker/tree/master/modules/matches101meta) module. This module aims to execute all the rules, that neither have a predicate nor a fragment as part of the constraint. The result is stored in files with the ending `*.matches.json` in [101web/resources location](http://data.101companies.org/resources/).
Once that is done we start to execute different modules to analyze the files. The way of the execution, depends on the metadata that are delivered about the file. The [metrics101meta](https://github.com/101companies/101worker/wiki/Module%20metrics101meta) module uses the GeSHi coded retrieved before to extract token sequences.

Another part in processing the [Language:101meta](#Language:101meta) is that we want to validate the found metadata especially the language. Therefore the rules identify by the suffix of a file not just a language but also a specific [validator](https://github.com/101companies/101worker/tree/master/validators). These [validators](https://github.com/101companies/101worker/tree/master/validators) are used to make sure that the assumed language is right. The specific [validators](https://github.com/101companies/101worker/tree/master/validators), are references over the validator metadata key and are finally executed in the [validate101meta](https://github.com/101companies/101worker/tree/master/modules/validate101meta) module.

```
{ 
  "suffix" : ".java", 
  "metadata" : { "validator" : "JValidator" }
}
```

Next we describe what is going to happen is a fact extraction on the files. This will take place in the [extract101meta](https://github.com/101companies/101worker/tree/master/modules/extract101meta) module. There are several kinds of [extractors](https://github.com/101companies/101worker/tree/master/extractors) that are plugged into the [101worker](https://github.com/101companies/101worker/) and are referenced over the [language keyword](* **language**) inside the metadata values. The [extract101meta](https://github.com/101companies/101worker/tree/master/modules/extract101meta) module will now simply observe the language of an artefact, by using the result of [matches101meta](https://github.com/101companies/101worker/tree/master/modules/matches101meta) module, and execute the right [extractor](https://github.com/101companies/101worker/tree/master/extractors) module.


Afterwards [fragmentMetrics101meta](https://github.com/101companies/101worker/tree/master/modules/fragmentMetrics101meta) module will do further processing of on the files using the previous result.


After the first analysis on the metadata the [101worker](https://github.com/101companies/101worker) will now continue to run the remaining rules. First the predicate rules will be executed. Rules of that type refer to a predicate that will be executed. Some predicates use the result of the previous fact extraction hence that module has a dependency to [extract101meta](https://github.com/101companies/101worker/tree/master/modules/extract101meta). Further this rules are not run on every file but just the once that have the requested language. An example of the discussed rule type is: 

```
{
	"predicate": "javaImport", 
	"args": ["javax.xml.bind"], 
	"language": "Java", 
	"metadata": {
		"comment": "API usage of JAXB", 
		"dependsOn": „JAXB"
	}
}
```

A more detailed description of the modules can be found in the [101worker](https://github.com/101companies/101docs/tree/master/worker/modules).


## Metadata scenarios

We introduce [Language:101meta](# Language:101meta) here by a series of examples that illustrate essential metadata scenarios in the [101project](http://101companies.org/wiki/@project).

### Inside Rules

The metadata can be found, as discussed above, either as a constraint or as a result. First of all we will cover an example of a metadata key as a constraint.

Constraint example:

```
{
	"predicate": "javaImport", 
	"args": ["javax.xml.bind"], 
	"language": "Java", 
	"metadata": {
		"comment": "API usage of JAXB", 
		"dependsOn": "JAXB"
	}
}
```

In that example there is a constraint of the `language` metadata-key. It is used to make sure that the rule is just applied on files that have the specific language. That is especially useful here because the technology [JAXB](https://jaxb.java.net/) is just used in a Java context and therefore there is no need to execute the predicate for all kinds of files.
Right now you cannot refer to every metadata-key. So if you plan to use it in another form than the one shown above, make sure that the [meta101 library](https://github.com/101companies/101worker/tree/master/libraries/101meta) already supports it. If not you could add it yourself.  


### Language-related metadata [[2]](http://101companies.org/wiki/Language:101meta) 

The first example concerns matching of files with suffix `.java` to be associated with the language `Java`.

```
{ 
  "suffix" : ".java", 
  "metadata" : { "language" : "Java" } 
}
```

The suffix constrains the suffix (the extension) of files to be matched. Metadata takes the form of a key-value pair with `language` as key and `Java` as value. In a conceptual sense, such metadata submits that the file in question is an element of the language specified; see the `elementOf` relationship of `Language:MegaL`. We assume that a 101companies-specific interpreter, such as the `101companies:Explorer`, links the key-value pair to the resource `Language:Java` as it is manifest on the `101wiki`.

The example specifies a single rule. In general, an [Language:101meta](#Language:101meta) specification is a list of rules. Here is a specification with two rules to match both `Language:JavaScript` and `Language:Java` files; array notation is used to this end:

```
[ 
  { 
    "suffix" : ".java", 
    "metadata" : { "language" : "Java" } 
  },
  { 
    "suffix" : ".js", 
    "metadata" : { "language" : "JavaScript" } 
  },
]
```

### Technology-related metadata [[3]](http://101companies.org/wiki/Language:101meta)

We will be concerned now with technologies as opposed to languages. We define rules related to the parser generator [Technology:ANTLR](http://www.antlr.org/) for illustration. In the case of using `ANTLR` with Java, the technology is packaged as a `.jar` archive. Hence, let us associate, for example, the (version-specific) file `antlr-3.2.jar` with the technology `ANTLR`.

```
{ 
  "basename" : "antlr-3.2.jar",
  "metadata" : { "partOf" : "ANTLR" } 
}
```

The basename constraint implies that we do not care about the directory of the matched file here. Metadata takes the form of a key-value pair with `partOf` as key and `ANTLR` as value. We use `partOf` here in the sense that a concrete artifact, such as a `.jar` archive, can be considered part of a technology, which is a conceptual (abstract) entity; see the `partOf` relationship of `Language:MegaL`. We assume that a 101companies-specific interpreter, such as the `101companies:Explorer`, links the value `ANTLR` to `Technology:ANTLR` as it is manifest on the `101wiki`.

Let us cover two versions of `ANTLR`:

```
[ 
  {
    "basename" : "antlr-2.7.6.jar",
    "metadata" : { "partOf" : "ANTLR" } 
  },
  { 
    "basename" : "antlr-3.2.jar",
    "metadata" : { "partOf" : "ANTLR" } 
  }
]
```

The example applies the same metadata to two different files. For conciseness' sake, the constraint keys for file matching (i.e., suffix and basename) may also be associated with lists of alternatives for matching. Thus, the two rules may be factored into one as follows:

```
{ 
  "basename" : [ "antlr-2.7.6.jar", "antlr-3.2.jar" ],
  "metadata" : { "partOf" : "ANTLR" } 
}
```

We may also use regular expression matching on file names. In this manner, we can even match all possible versions of `ANTLR` with a single rule. To this end, we allow for any substring between `antlr-` and `.jar`. Thus:

```
{ 
  "basename" : "#^antlr-.*\.jar$#",
  "metadata" : { "partOf" : "ANTLR" } 
}
```

Here, `^` marks the beginning of the string, `$` marks the end of the string, and `\` escapes a metasymbol (because `.` is metasymbol for any character). The regular expression is enclosed by `#...#` thereby expressing unambiguously that regular expression matching as opposed to literal name matching is to be applied.

The `.jar` file for ANTLR is by no means the only way how files can be associated with `ANTLR`. In general, `technologies` deal with various kinds of files: 

* input 
* output 
* configuration files
* ... 

For example `.g` files, which are an indicator of `ANTLR` usage because `ANTLR grammar` files use this extension:

```
{ 
  "suffix" : ".g",
  "metadata" : { "inputOf" : "ANTLR" }
}
```

This time, the metadata declares that the given file is input for the parser generator `ANTLR`. We assume that a 101companies-specific interpreter, e.g., `101companies:Explorer` for the exploration of contributions, prioritizes input files over output files such as generated source code that is not meant for human consumption. The use of `ANTLR` may also be inferred on the grounds of generated files. When `ANTLR` is used in a common manner, then generated code parser and lexer are to be found in files with specific names as follows:

```
{ 
  "basename" : [ "#^.*Parser\\.java$#", "#^.*Lexer\\.java$#" ],
  "metadata" :  { "outputOf" : "ANTLR" }
}
```


*TODO: one may attempt a simplification of the patterns. Hint: the beginning of the file does not need to be matched explicitly.) This time, the files at hand are tagged as resulting from the application of `ANTLR` as an output. We assume that a 101companies-specific interpreter, e.g., `101companies:Explorer` for the exploration of contributions, de-prioritizes `output` files as opposed to `input` files.*


There is a major problem with the rule for generated files: the rule relies on insufficiently distinctive filename patterns. The use of `Parser` or `Lexer` in naming source files for parsers and lexers does not reasonably imply usage of `ANTLR`. Thus, we need to further constrain the rule in a way that the content of the files can be checked to support the assumption about `ANTLR` usage. We will return to this problem later in the context of a more complete discussion of metadata mechanics TODO.


### Feature-related metadata: [[4]](http://101companies.org/wiki/Language:101meta)

We may want to `tag` files with features of the `101system`, as they are implemented in the file. The following example deals with `Contribution:javaStatic`, which is a simple and modular Java-based implementation of the 101system:
```
[ 
  { 
    "filename" : "contributions/javaStatic/org/softlang/model/Company.java",
    "metadata" : { "feature" : "Tree structure" } 
  },
  { 
    "filename" : "contributions/javaStatic/org/softlang/model/Department.java",
    "metadata" : { "feature" : "Tree structure" } 
  },
  { 
    "filename" : "contributions/javaStatic/org/softlang/model/Employee.java",
    "metadata" : { "feature" : "Tree structure" } 
  },
  { 
    "filename" : "contributions/javaStatic/org/softlang/behavior/Total.java",
    "metadata" : { "feature" : "Type-driven query" } 
 },
  { 
    "filename" : "contributions/javaStatic/org/softlang/behavior/Cut.java",
    "metadata" : { "feature" : "Type-driven transformation" } 
  }
]
```

### Domain-related metadata

We may also want to `tag` files with terms of the `101companies:Vocabulary` which collects nouns and verbs of the `101companies domain`. This may be, in fact, an alternative to tagging files with features. For instance, we may want to express that certain modules define the 101companies-specific operations `101term:Cut` and `101term:Total`. Again, we apply tagging to `Contribution:javaStatic`.

```
[ 
  { 
    "filename" : "contributions/javaStatic/org/softlang/behavior/Total.java",
    "metadata" : { "term" : "Total" } 
  },
  { 
    "filename" : "contributions/javaStatic/org/softlang/behavior/Cut.java",
    "metadata" : { "term" : "Cut" } 
  }
]
```

One may think of these 101companies-specific tags as being more concise than the feature-oriented tags that we used earlier. That is, term `101term:Total` is a proxy for feature `Feature:Total` and term `101term:Cut` is a proxy for feature `Feature:Cut`. As a guideline, such concise terms are to be preferred over features for tagging, whenever applicable.

We continue the previous example, by tagging also structure-related modules. That is, we associate the tags for the terms `101term:Company`, `101term:Department` and `101term:Employee` with the appropriate `.java` files. Incidentally, such tagging is more precise than the earlier tagging with the feature `Feature:Hierarchical` company, which did not distinguish the different domain concepts for `companies`, `departments`, and `employees`.

```
[ 
  { 
    "filename" : "contributions/javaStatic/org/softlang/model/Company.java",
    "metadata" : { "term" : "Company" } 
  },
  { 
    "filename" : "contributions/javaStatic/org/softlang/model/Department.java",
    "metadata" : { "term" : "Department" } 
  },
  { 
    "filename" : "contributions/javaStatic/org/softlang/model/Employee.java",
    "metadata" : { "term" : "Employee" } 
  }
]
```

Terms can also be composed to provide more accurate descriptions. For instance, we may want to express that the module for cutting salaries actually does so by breaking down functionality into cutting company objects, department objects, and employee objects. Thus:

```
[ 
  { 
    "filename" : "contributions/javaStatic/org/softlang/behavior/Cut.java",
    "metadata" : { "phrase" : ["Cut", "Company"] } 
  },
  { 
    "filename" : "contributions/javaStatic/org/softlang/behavior/Cut.java",
    "metadata" : { "phrase" : ["Cut", "Department"] } 
  },
  { 
    "filename" : "contributions/javaStatic/org/softlang/behavior/Cut.java",
    "metadata" : { "phrase" : ["Cut", "Employee"] } 
  }
]
```

Such phrases are even more useful when attached to specific file fragments as opposed to entire files. We will return to this opportunity later in the context of a more complete discussion of metadata mechanics.

### Concept-related metadata

Further, we may also want to `tag` files with any concepts in the broader areas of software technologies and software languages. Ideally, such concepts should be readily modeled on the 101wiki. For instance, we may want to express that certain modules define a parser, a GUI, or use a MVC architecture.

Consider again Contribution:antlrObjects which clearly contains program components for parsing and lexing. Accordingly, we tag the corresponding files:

```
[
  { 
    "filename" : "contributions/antlrObjects/org/softlang/parser/CompanyParser.java",
    "metadata" :  { "concept" : "Parser" }
  },
  { 
    "filename" : "contributions/antlrObjects/org/softlang/parser/CompanyLexer.java",
    "metadata" :  { "concept" : "Lexer" }
  }
]
```

In this context, if not earlier, the question may arise as to whether tags may also be associated automatically on the grounds of data mining techniques. That is, some [Language:101meta](#Language:101meta) does not need to be authored if it may be inferred. This is clearly possible for domain terms and concepts and even features. The [101project](http://101companies.org/wiki/@project) involves related efforts.

## Processing-related metadata

### Validators:

As another form of metadata, a [validator](https://github.com/101companies/101worker/tree/master/validators) may be associated with each file. The meaning of validation is here that matched files are to be validated to essentially verify assumptions implied by matching. For instance, we can be reasonably sure that files with suffix `.java` contain `Java` source code, but if we wanted to validate this assumption, then we may register a [validator](https://github.com/101companies/101worker/tree/master/validators). 
To avoid Code-Injection and make the architecture of the [101worker](https://github.com/101companies/101worker) cleaner these executables are plugged into the  [101worker dir](https://github.com/101companies/101worker/tree/master/validators) and can be referenced over language. Right now follow validators are avaliable:

* [CSharp](https://github.com/101companies/101worker/tree/master/validators/CSharp) files with suffix `.cs` contain `C#`.
* [HTML](https://github.com/101companies/101worker/tree/master/validators/HTML) files with suffix `.html` contain `JTidy`.
* [Java](https://github.com/101companies/101worker/tree/master/validators/Java) files with suffix `.java` contain `Java`.
* [CSS](https://github.com/101companies/101worker/tree/master/validators/CSS) files with suffix `.html` contain `CSS`.
* [XML](https://github.com/101companies/101worker/tree/master/validators/XML) files with suffix `.xml` contain `XML`.
* [XSD](https://github.com/101companies/101worker/tree/master/validators/XSD) files with suffix `.xsd` contain `XSD`.

If there is no validator for the language in question the validation proccess won't take place.

A [validator](https://github.com/101companies/101worker/tree/master/validators) is an executable that is applied to the file in question. It can be coded in any language of choice as long as the name of the executable is `validator`. The [validate101meta module](https://github.com/101companies/101worker/tree/master/modules/validate101meta)  is responsible for excecuting the validators. It decides which validator to execute by the metadate-language key. Therefore the follow rule, that happens to assign the language Java to files with the ending ".java", will execute the validator for the Java language:

```
{ 
  "suffix" : ".java", 
  "metadata" : { "language" : "Java" }
} 
```

A validator essentially parses the source code, it does not attempt compilation and it does not enforce any static semantics rules. Zero exit code is to be interpreted as successful validation and non-zero exit code as failure. Validation must not be confused with the predicate form of constraint as validation is applied past successful rule matching, whereas constraint checking is part of matching itself. The `101companies:Explorer` leverages validation in a manner, that all failed validation is highlighted to receive the attention of the user, thereby suggesting eventual revision of the relevant rule for matching or making a change to the relevant file or its filename.

## extractors:

A fact [extractor](https://github.com/101companies/101worker/tree/master/extractors) may be associated with each file. The `language` metadata key is used to determine what fact [extractor](https://github.com/101companies/101worker/tree/master/extractors) should be executed. All available extractors can be found in the [extractor](https://github.com/101companies/101worker/tree/master/extractors) direction of the [101worker](https://github.com/101companies/101worker) main folder and will be called by the [extract101meta](https://github.com/101companies/101worker/tree/master/modules/extract101meta) module.  They can be coded in any language. 

There is one [extractor](https://github.com/101companies/101worker/tree/master/extractors) for supporting ten different types of source code files:

* [../CSharp/extractor](https://github.com/101companies/101worker/tree/master/extractors/CSharp) extracts `C#` files.
* * [../CSS/extractor](https://github.com/101companies/101worker/tree/master/extractors/CSS) extracts `CSS` files.
* [../HTML/extractor](https://github.com/101companies/101worker/tree/master/extractors/HTML) extracts `HTML` files.
* [../Haskell/extractor](https://github.com/101companies/101worker/tree/master/extractors/Haskell) extracts `Haskell` files.
* [../JSON/extractor](https://github.com/101companies/101worker/tree/master/extractors/JSON) extracts `JSON` files.
* [../Java/extractor](https://github.com/101companies/101worker/tree/master/extractors/Java) extracts `Java` files.
* [../JavaScript/extractor](https://github.com/101companies/101worker/tree/master/extractors/JavaScript) extracts `JavaScript` files.
* [../Python/extractor](https://github.com/101companies/101worker/tree/master/extractors/Python) extracts `Python` files.
* [../SQL/extractor](https://github.com/101companies/101worker/tree/master/extractors/SQL) extracts `SQL` files.
* [../XMI/extractor](https://github.com/101companies/101worker/tree/master/extractors/XMI) extracts `XMI` files.
* [../XSD/extractor](https://github.com/101companies/101worker/tree/master/extractors/XSD) extracts `XSD` files.

In this manner, files may be processed by fact extractors and thereby enable further functionality. For instance, we may assume that the fact [extractor](https://github.com/101companies/101worker/tree/master/extractors) determines all imports made by some source code so that rules for constraining imports may rely on such facts as opposed to performing text matching of fact extraction themselves.

### Inside Module Description:

The different modules depend on certain metadata. For example uses the [extract101meta](https://github.com/101companies/101worker/tree/master/modules/extract101meta) module the retrieved languages to determine which [extractor](https://github.com/101companies/101worker/tree/master/extractors) it should execute. This is the case since every language has a specific [extractor](##extractors). 
That is the reason why every module that is involved with the processing of metadata has to insert specific values into the module description. First of all it has to tell the [runner](https://github.com/101companies/101worker/tree/master/tools/runner) which metadata the module depends on and which one is obtained (e.g matches receives: language). The reason we know that is the simple fact that there are specific patterns due to the architecture of rules. So it is safe to say that the suffix declares the language.

The [predicates101meta](https://github.com/101companies/101worker/blob/master/modules/predicates101meta/) module, is an example of a how the [Language:101meta](#Language:101meta) is embedded in the modul description:

```
"metadata" : { 
	"dependencies" : 	["language"], 
	"obtained" :		["comment", "feature", "dependsOn"] 
			}
``` 
source: https://github.com/101companies/101worker/blob/master/modules/predicates101meta/module.json


### Table with different kinds of dependencies 

|			|Suffix		|filename	|dirname|basename	|predicate args	|fragment (+filename)	|content(+basename or suffix)	|
|-----------|-----------|-----------|-------|-----------|---------------|-----------------------|-------------------------------|
|language	|b			|			|		|ok			|				|						|								|
|geshi		|b			|			|		|b			|				|						|								|
|validator	|b			|			|		|			|				|						|								|
|nature		|b			|			|		|			|				|						|								|
|comment	|db			|b			|b		|dok		|db				|						|b								|
|manual		|			|b			|		|			|				|						|								|
|feature	|			|b			|		|			|				|						|								|
|relevance	|			|db			|b		|g			|g				|						|b								|
|phrase		|			|o			|		|			|				|b						|								|
|feature?	|			|g			|		|			|db				|						|								|
|term		|			|			|		|			|				|						|								|
|dominator	|			|r			|		|dok		|				|						|								|
|[extractor](https://github.com/101companies/101worker/tree/master/extractors)	|b			|			|		|			|				|						|								|
|concept	|db			|			|		|			|				|						|b								|
|dominator?	|db			|db			|		|			|				|						|								|
|assignment	|			|db			|db		|			|				|						|								|
|dependsOn	|			|			|		|b			|b				|						|								|
|outputOf	|			|			|		|			|				|						|b								|

### Inside Predicates

Every predicate has a predicate description that informs the [predicates101meta](https://github.com/101companies/101worker/tree/master/modules/predicates101meta) module about the module name, the metadata as well as modul dependencies and the minimum and maximum arguments. Although it can depend on metadata there is right now no way to tell what metadata the predicates obtains. We decided against giving that option, because right now there is no use case where that option would be of any use.

```
{
	"name" : "NamePredicate", 	
	"args" : [minimum number of arguments, maximum number of arguments], 
	"dependencies" : ["extract101meta"], 
	"metadata" : [] 	
 }
```
Further reading: [predicates description](https://github.com/101companies/101worker/blob/master/predicates/README.md)
