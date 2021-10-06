---
title: "About"
---

## Hi, I'm Brendan Griffen

Hi. I’m Brendan, a computational astrophysicist by training with core interests in biology, engineering and computer science based between San Francisco, California and Brisbane, Australia.

I'm currently Chief Technology Officer and co-founder at [Dynomics](https:/www.dynomics.com), a startup focused on reversing heart failure by deploying complex *in silico* and *in vitro* systems at the intersection of computer science, machine learning, biology and bioengineering.

In a past life, I was a computational astrophysicist and Project Lead on the [Caterpillar Project](https://www.caterpillarproject.org/), a large computational galaxy formation suite at MIT focusing on the origin and evolution of the Milky Way as well as project collaborator on the more general galaxy formation project of [Illustris](http://www.illustris-project.org/) at the Center for Astrophysics at Harvard University.

### Code

I've written a bit of software over the years. My public projects can be found in
[repositories on GitHub](https://github.com/bgriffen). Examples include:

<script id="code-template" type="x-tmpl-mustache">
{{#codes}}
<li>
    <i><a href="{{url}}">{{name}}</a></i> &mdash; {{description}}
</li>
{{/codes}}
{{^codes}}
Unable to load of software.
{{/codes}}
</script>

<ul id="codelist"></ul>

More tools, especially bio focused, will be available soon.

### Affiliations

I've also written a lot of code for other teams I've been apart of:

* [*Dynomics Inc*](https://github.com/orgs/dynomics/) &mdash; discovering targeted therapies to reverse heart failure using cardiac organoids.
* [*Scaled Biolabs Inc*](https://github.com/orgs/scaledbiolabs/) &mdash; combining microfluidics, bioengineering and computer science to do high-throughput optimization.
* [*Caterpillar Project*](https://github.com/orgs/caterpillarproject/) &mdash; large scale dark matter simulations to disentangle the formation history of the Milky Way.

### Publications

My full list of publications is available
[Google Scholar](https://scholar.google.com.au/citations?user=ndwtPccAAAAJ&hl=en)
but here are a few highlights:

#### Biotechnology

<script id="pub-template-biotech" type="x-tmpl-mustache">
{{#pubsother}}
<li>
    {{authorsFormat}}, {{year}}, <a href="{{url}}"><i>{{title}}</i></a>, {{pub}}.
    {{#codeLink}}<br><small>[<a href="{{codeLink}}">code</a>]</small>{{/codeLink}}
</li>
{{/pubsother}}
{{^pubsother}}
Unable to load publication list.
{{/pubsother}}
</script>

<ul id="publist-biotech"></ul>

#### Astrophysics
<script id="pub-template-astro" type="x-tmpl-mustache">
{{#pubs}}
<li>
    {{authorsFormat}}, {{year}}, <a href="{{url}}"><i>{{title}}</i></a>.
    {{#codeLink}}<br><small>[<a href="{{codeLink}}">code</a>]</small>{{/codeLink}}
</li>
{{/pubs}}
{{^pubs}}
Unable to load publication list.
{{/pubs}}
</script>

<ul id="publist-astro"></ul>



<script src="https://unpkg.com/mustache@latest"></script>
<script>
  var codeMap = {
  };

  function formatAuthors(authors) {
    authors = authors.map(author => {
      var tokens = author.split(", ");
      if (tokens.length != 2) return author;
      return tokens[1][0] + ". " + tokens[0];
    });
    if (authors.length == 1) {
      return authors[0];
    } else if (authors.length >= 5) {
      return authors.slice(0, 4).join(", ") + ", et al.";
    }
    return authors.slice(0, authors.length - 1).join(", ") + ", and " + authors[authors.length - 1];
  }

  (() => {
    var codeTemplate = document.getElementById("code-template").innerHTML;
    fetch("https://raw.githubusercontent.com/bgriffen/cv/main/data/repos.json")
      .then(response => response.json())
      .then(data => {
        data = data.data.user.pinnedItems.edges.map(value => value.node);
        var rendered = Mustache.render(codeTemplate, { codes: data });
        document.getElementById("codelist").innerHTML = rendered;
      })
      .catch(() => {
        var rendered = Mustache.render(codeTemplate, { codes: [] });
        document.getElementById("codelist").innerHTML = rendered;
      });

    var pubTemplateastro = document.getElementById("pub-template-astro").innerHTML;
    fetch("https://raw.githubusercontent.com/bgriffen/cv/main/data/pubs.json")
      .then(response => response.json())
      .then(data => {
        // Only first author
        data = data.filter(value => {
          return value.authors[0].startsWith("Griffen") && value.doctype == "article";
        });

        // Format authors
        data = data.map(value => {
          value.authorsFormat = formatAuthors(value.authors);
          value.codeLink = codeMap[value.doi];
          return value;
        });

        var rendered = Mustache.render(pubTemplateastro, { pubs: data });
        document.getElementById("publist-astro").innerHTML = rendered;
      })
      .catch(() => {
        var rendered = Mustache.render(pubTemplateastro, { pubs: [] });
        document.getElementById("publist-astro").innerHTML = rendered;
      });

    var pubTemplatebiotech = document.getElementById("pub-template-biotech").innerHTML;
    fetch("https://raw.githubusercontent.com/bgriffen/cv/main/data/other_pubs.json")
      .then(response => response.json())
      .then(data => {
        // Only first author
        data = data.filter(value => {
          return value.authors[0].startsWith("Mills") && value.doctype == "article";
        });

        // Format authors
        data = data.map(value => {
          value.authorsFormat = formatAuthors(value.authors);
          value.codeLink = codeMap[value.doi];
          return value;
        });

        var rendered = Mustache.render(pubTemplatebiotech, { pubsother: data });
        document.getElementById("publist-biotech").innerHTML = rendered;
      })
      .catch(() => {
        var rendered = Mustache.render(pubTemplatebiotech, { pubsother: [] });
        document.getElementById("publist-biotech").innerHTML = rendered;
      });
  })();
</script>
