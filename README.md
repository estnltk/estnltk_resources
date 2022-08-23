# EstNLTK's resources index

This repository contains the index of EstNLTK's downloadable resources.

The file `resources_index.json` should have the following format:

```yaml
{
    "resources": [
        {
            # specific name of the resource. must be unique. mandatory
            "name": ___,

            # alternative names for the resource. recommended
            "aliases": [ ..., ... ], 

            # (long) description of the resource. recommended
            "desc": ___,

            # license. can be freeform text. recommended
            "license": ___, 

            # download url of the resource. zip or gz file or huggingface repo. mandatory
            "url": "https://...", 

            # size of the unpacked resource in GB. floating point number. recommended
            "size": ___,

            # MD5 hexdigest string of the packed resource. recommended
            "md5": ___,
          
            # path of the resource inside the resources folder once it has been unpacked. 
            # must be unique. mandatory
            "unpack_target_path": ___, 

            # path to the resource inside the archive. optional
            "unpack_source_path": ___, 

            # revision of a huggingface repo (if url is a huggingface repo). optional.
            "hf_revision": ___,

            # estnltk version specifier, following PEP 440 format. optional.
            "estnltk_version": ___, 

            # estnltk_neural version specifier, following PEP 440 format. optional.
            "estnltk_neural_version": ___, 
        },

        ...

    ]
}
``` 

Notes:

* `"name"` must contain a date in the ISO format (`YYYY-MM-DD`), e.g. `2021-05-29` or `2015-06-21`. this date is used for sorting resources, and by default, only the resource with the latest date is downloaded and used by tools;
* `"aliases"` can list names of the tool or tagger that uses the resource, e.g. different stanza syntax models can be marked with aliases `"stanza_syntax"`, `"stanzasyntaxtagger"`;  
* `"url"` is the web address from where the resource can be downloaded and extracted. It can be:
	* a zip file containing the resource;
	* a gz file containing the resource (excluding .tar.gz which unpacking is not implemented);
	* a full address of a huggingface model repository, e.g. `"https://huggingface.co/tartuNLP/EstBERT"`. So, the  resource can be downloaded from a huggingface repository. You can use optional attribute `"hf_revision"` to pinpoint a specific branch, tag, or commit hash of the repository. Note that only _full repository download_ is supported: a resource cannot be a single file or a directory inside a huggingface repository.
* `"size"` gives the size of the unpacked resource in GB, e.g. `0.232` stands for `232 MB` and `1.6` stands for `1.6 GB`. This information is prompted to the user before the download, so that the user can decide whether she/he wants to download a large resource;
* `"md5"` gives MD5 hexdigest string of the packed resource. The digest is checked after downloading an archive and if the sums do not match, the resource will be discarded. Note that this check is not implemented for huggingface resources. 
* `"unpack_target_path"` must be a valid path name, corresponding to a file (if the package contains a single one file) or a directory (directory which needs to be extracted from the zip). this path is used for checking the existence of the resource once it has been downloaded and unpacked. Notes: 
	* use `/` as a directory separator;
	* if the resource is a directory, then the path should end with `/`; otherwise, it is assumed to be path of a file;
	* the path (even if it is a file path) should contain two directories: first directory names the tool using the resource, and its sub diretory names the specific version of the model. Examples: `"stanza_syntax/models_2020-11-30/"`, `"udpipe_syntax/models_2021-05-29/"`, `"word2vec/embeddings_2015-06-21/lemmas.cbow.s100.w2v.bin"`;
* `"unpack_source_path"` can be used to specify the resource path inside the archive iff the archive has different directory structure. However, you should not use that often. It is recommended to pack resources in a way that the archive  contains the same directory structure as specified by `"unpack_target_path"`;
* `"estnltk_version"` and `"estnltk_neural_version"` can be used to specify, which version of estnltk or estnltk_neural is required by the resource. Must follow the _version specifier format_ defined in [PEP 440](https://peps.python.org/pep-0440/). Examples: `>=1.7.1` (a single version clause), `>1.6.9.1, <=1.7.5` (multiple clauses separated by comma). The current implementation of version checking is rudimentary: only operators `==`, `<=`, `>=`, `<`, `>` can be used for version comparison, only canonical version strings are permitted (trailing `.*` is not allowed), and the comparison works on final release segments of versions (it is not expected to work properly on pre-release, post-release etc. segments).

##### How to test my resource index update?

Before committing an update to the resource index, test it out with EstNLTK. 
This can be done with EstNLTK's version 1.7.0+.
First, copy the updated index json file to [EstNLTK's resources directory](https://nbviewer.org/github/estnltk/estnltk/blob/58ac622ba70cd83b70775315f6518246780631ac/estnltk/tutorials/estnltk_resources.ipynb#Where-is-the-resources-directory-and-how-to-change-it?). Then use functions [download and get\_resource\_paths](https://nbviewer.org/github/estnltk/estnltk/blob/58ac622ba70cd83b70775315f6518246780631ac/estnltk/tutorials/estnltk_resources.ipynb#In-a-nutshell) to test if  downloading and accessing the resource works as expected.    
 

**future work**

**TODO**: index entries inside `resources_index.json` should be placed into separate directories (a directory for each tool or tagger, e.g. `stanza_syntax`, `word2vec_embeddings`) as json files (e.g. `stanza_syntax_2020-11-30.json`, `stanza_syntax_2021-05-29.json` etc);

**TODO**: a script/workflow should be developed that traverses all directories, reads data from json files and builds `resources_index.json` automatically;
