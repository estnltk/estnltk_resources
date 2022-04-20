# EstNLTK's resources index

This repository contains the index of EstNLTK's downloadable resources.

The file `resources_index.json` should have the following format:

```yaml
{
    "resources": [
        {
            # specific name of the resource. must be unique. mandatory
            "name": ___,

            # alternative names for the resource. optional
            "aliases": [ ..., ... ], 

            # (long) description of the resource. optional
            "desc": ___,

            # license. can be freeform text. optional?
            "license": ___, 

            # download url of the resource. zip or gz file or huggingface repo. mandatory
            "url": "https://...", 

            # size of the unpacked resource in GB. floating point number. optional.
            "size": ___,

            # MD5 hexdigest string of the packed resource. optional.
            "md5": ___,
          
            # path of the resource inside the resources folder once it has been unpacked. 
            # must be unique. mandatory
            "unpack_target_path": ___, 

            # path to the resource inside the archive. optional
            "unpack_source_path": ___, 

            # revision of a huggingface repo (if url is a huggingface repo). optional.
            "hf_revision": ___,

        },

        ...

    ]
}
``` 

Notes:

* `"name"` must contain an ISO-format date, e.g. `2021-05-29` or `2015-06-21`. this date is used for sorting resources, and by default, only the resource with the latest date is downloaded and used by tools;
* `"aliases"` can list names of the tool or tagger that uses the resource, e.g. different stanza syntax models can be marked with aliases `"stanza_syntax"`, `"stanzasyntaxtagger"`;  
* `"url"` must point to a zip or gz file, which can be downloaded and unpacked. Other archive formats (including .tar.gz) are not supported;
	* `"url"` can also be a full address of a huggingface model repository, e.g. `"https://huggingface.co/tartuNLP/EstBERT"`. Then the repository will be downloaded as a resource. You can use optional attribute `"hf_revision"` to pinpoint a specific branch, tag, or commit hash of the repository. Note that only _full repository download_ is supported: a resource cannot be a single file or a directory inside a huggingface repository.
* `"size"` gives the size of the unpacked resource in GB, e.g. `0.232` stands for `232 MB` and `1.6` stands for `1.6 GB`. This information is prompted to the user before the download, so that the user can decide whether she/he wants to download a large resource;
* `"md5"` gives MD5 hexdigest string of the packed resource. The digest is checked after downloading an archive and if the sums do not match, the resource will be discarded. Note that this check is not implemented for huggingface resources.
* `"unpack_target_path"` must be a valid path name, corresponding to a file (if the package contains a single one file) or a directory (directory which needs to be extracted from the zip). this path is also used for checking the existence of the resource once it has been downloaded and unpacked. 
	* use `/` as a directory separator;
	* if the resource is a directory, then the path should end with `/`; otherwise, it is assumed to be path of a file;
	* the path (even if it is a file path) should contain two directories: first directory names the tool using the resource, and its sub diretory names the specific version of the model. Examples: `"stanza_syntax/models_2020-11-30/"`, `"udpipe_syntax/models_2021-05-29/"`, `"word2vec/embeddings_2015-06-21/lemmas.cbow.s100.w2v.bin"`;
* `"unpack_source_path"` can be used to specify the resource path inside the archive iff the archive has different directory structure. However, it is recommended that the archive contains the same directory structure as  specified by `"unpack_target_path"`;


**future work**

**TODO**: index entries inside `resources_index.json` should be placed into separate directories (a directory for each tool or tagger, e.g. `stanza_syntax`, `word2vec_embeddings`) as json files (e.g. `stanza_syntax_2020-11-30.json`, `stanza_syntax_2021-05-29.json` etc);

**TODO**: a script/workflow should be developed that traverses all directories, reads data from json files and builds `resources_index.json` automatically;
