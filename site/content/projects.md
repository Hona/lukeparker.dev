+++
title = "Projects"
slug = "projects"
+++

> Using GitHub API

### Starred

{{< rawhtml >}}
<div id="github-projects"><i>Loading projects using GitHub API...</i></div>
<script async defer>
var element = document.getElementById("github-projects");

function Get(yourUrl){
    var Httpreq = new XMLHttpRequest(); // a new request
    Httpreq.open("GET",yourUrl,false);
    Httpreq.send(null);
    return Httpreq.responseText;          
}

var json_obj = JSON.parse(Get("https://gh-pinned-repos-5l2i19um3.vercel.app/?username=Hona"));

element.outerHTML = json_obj.map(function(x) { return "<p>" + "<strong>" + "<a href='" + x.link + "'>" + x.owner + "/" + x.repo + "</a>" + "</strong> - " + x.stars + " &#9734; - " + x.language + "<br />" + x.description + "</p>"})
    .reduce((acc, curr) => acc + curr);

</script>
{{< /rawhtml >}}

### Recent Contributions

These are listed on my GitHub profile under 'Contribution activity', [here](https://github.com/Hona)
