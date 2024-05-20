---
title: "Validate research statement"

# research:
#   institute: Georgia Institute of Technology
#   PI: Dr. Jun Ueda
#   date: November 2022
#   funding:
#     body:  National Science Foundation
#     grant: 2112793
#   publication:
#     title: Secure Teleoperation Control Using Somewhat Homomorphic Encryption
#     url: https://doi.org/10.1016/j.ifacol.2022.11.247

# DYERS vs BFV True metadata
research:
  institute: Georgia Institute of Technology
  PI: Dr. Jun Ueda
  date: November 2022
  funding:
    - body:  National Science Foundation
      grant: 2112793
    
    - body: Japan Society for the Promotion of Science KAKENHI
      grant: JP22H01509
  publication:
    title: Secure Teleoperation Control Using Somewhat Homomorphic Encryption
    url: https://doi.org/10.1016/j.ifacol.2022.11.247
---

**Disclosure of Funding Source:** 
This research was conducted at the {{page.research.institute}} 
under the supervision of {{page.research.PI}}, completing in {{page.research.date}},
resulting in the following publication: [{{page.research.publication.title}}]({{page.research.publication.url}}).
<br/>
<br/>
This study was supported in part by: {% for record in page.research.funding %}{{record.body}} Grant Nos. {{record.grant}}{% unless forloop.last %}, {% endunless %}{% endfor %}.
Any opinions, findings, and conclusions or recommendations expressed in this material are those of the author and do
not necessarily reflect the views of the {{page.research.funding.body}}.
* thing 1
* thing 2
more text
{: .notice}