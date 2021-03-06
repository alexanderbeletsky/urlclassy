#URL Classy

Guessing a class for a URL only from its text.

## Description

URL Classy is a library that assigns a top-level dmoz category based on the URL text.  It is an implementation of URL-based classifier in JavaScript.

See [http://wiki.duboue.net/index.php/URL_Classifier](http://wiki.duboue.net/index.php/URL_Classifier)

**Work in progress**, not nicely packaged yet. Do not look at this code as an example of how to pack things.

To use it, download content.rdf.u8.gz from dmoz.org and extract a training file as follows:

```bash
$ zcat content.rdf.u8.gz | perl -ne 'if(m/<ExternalPage/){($u)=m/about=\"(.*)\"\>/}elsif(m/\<topic/){($t)=m/\<topic\>Top\/(.*)\<\/topic\>/}elsif(m/\<\/ExternalPage/){print "$t\t$u\n" if($t and $u);$t="";$u=""}'|sort|uniq | gzip - > full_cat_urls.tsv.gz
zcat full_cat_urls.tsv.gz | perl -ne '@a=split(/\t/,$_);@b=split(/\//,@a[0]);push@b,'Top'; print $b[0]."\t".$b[1]."\t".$a[1]' > two_cats_urls.tsv
```

(or download a snapshot from [http://aprendizajengrande.net/two_cats_urls.tsv.gz](http://aprendizajengrande.net/two_cats_urls.tsv.gz))

then train the classifier with

```bash
$node train/stay_classy.js two_cats_urls.tsv
```

full training using two level classes is currently running into out of memory problems (maybe related to [https://code.google.com/p/v8/issues/detail?id=847](https://code.google.com/p/v8/issues/detail?id=847) ?).

The current demo (in example/) is trained in top level class and 10%. Unseen performance is 54% accuracy but that includes lots of repeated URLs. Actual performance seems to be at 30% at most.

(The current training code uses only 1% of the data, change lines 77 and 81 to increase the percentage of the file to use.)

See [example](/example).

