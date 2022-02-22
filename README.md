# My markdowns for Firma Technologies

Here's all the documentation markdowns during my **Firma Technologies** internship.

Due to company confidentiality policy, I'm only able to publicly share the following files/repositories.

<br>

**This repo contains:**

- [Documentation for Meshlab](MESHLAB_DOCUMENTATION.md), an open source point cloud / 3D meshing software

- Documentation for [setting up our static frontend](SETTING_UP_FRONTEND.md) and for all its [files and functions](FRONTEND_DOCUMENTATION.md)

<br><br>

## Related repositories

Public repositories for libraries I worked on during my internship.

<br>

### [Modified CORS Anywhere proxy](https://github.com/EvitanRelta/cors-anywhere/tree/master)

A fork of **Rob--W's** [CORS Anywhere](https://github.com/Rob--W/cors-anywhere) that I've modified to circumvent CORS restrictions.

It's modified to escape [forbidden headers](https://fetch.spec.whatwg.org/#forbidden-header-name) when prefixed with a `-` as shown:

```
[Header]     : [Value]
-----------------------------
 -Origin      : 'https://example.com'
 -Referer     : 'https://example.com/subdirectory'
 -User-Agent  : 'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.131 Safari/537.36'
```

It was used to spoof Google Cloud ML into accepting a prediction request from an unauthorised website during my internship.

<br>

### [Polygon Pascal VOC Writer](https://github.com/EvitanRelta/polygon-pascalvoc-writer)

A Python module to generate Pascal VOC xml annotation files.

Improved upon **AndrewCarterUK's** [PASCAL VOC Writer](https://github.com/AndrewCarterUK/pascal-voc-writer).

It was used to generate annotation files to feed our Tensorflow ML model during my internship.