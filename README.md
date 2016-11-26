# voting-machines
A repo for exploring the software quality of electronic voting machines

This repository provides a breakdown of the software validation process as performed to certify electronic voting equipment. Each device's Certificate of Conformance and Final Test Report are included in the appropriate folder, organized by manufacturer. These reports are public records available through [the Election Assistance Commission's website](https://www.eac.gov/default.aspx) and are only provided for convenience; copyright on these documents belongs to the original holders, and no guarantee is made that these documents are current.

[Skip to the good stuff](#a-summary-of-the-software-review-process-for-all-certified-electronic-voting-systems)

## Voting Machines and Quality Assurance

Unlike several failure-intolerant devices, there is _no codified set of federal regulations_ governing the quality of electronic voting machines. There are, however, federally-recognized certification standards and federally-accredited testing facilities. Nonetheless, we require some commentary on this. Certification is voluntary for manufacturers; a handful of states have state-level regulations mandating federal certification. More on this later.

#### Non-technical Summary

Voting machines have a lot of components that need to be tested. Electrical systems, physical cases, security locks, scanners, speakers, and all that stuff has to meet certain engineering quality standards. Certification bodies exist to test these machines. However, software is a more mysterious being. Software verification is _very hard_; in fact, it's one of the hardest problems out there. Software can introduce lots of failures: it can change a vote, it can count a vote twice or not at all, it can lose votes. So one would expect that voting machine software is thoroughly checked and ensured that the answer it outputs is always the correct answer.

My findings indicate that this is not the case.

The software standards for voting machines mostly govern _code style_. Code style is about how the code is presented, and truly it is very important. However, code style is form, not function; it doesn't change how the code runs, just how humans read it. Some formatting problems can include things like "how long is this line?" or "is this line properly indented?"

The certification program effectively _only_ looks at code style. Furthermore, most of the style review was performed by automated tools, not humans. This means that backdoors or other attempts at deliberate malfeasance can be easy to sneak into the software, and it's possible that they won't be caught during the functionality testing portion of certification.

For instance, consider this block of pseudocode:

```C#
if (numVotesRecorded > 1000)
{
    if (vote.Candidate == "Smith")
        votes["Smith"] += 2;
    else
        votes["Smith"] += 1;
}
```

In such a case, votes for Candidate Smith get double-counted only when the machine has recorded at least 1000 votes. This would not get caught during an automated style review.

Most of the human-review steps only covered a small fraction of the code, and of those human reviews, most of the review focused only on _comments_, sections of the code used to explain what happens but isn't actually execued by the machine. Therefore, it's easy for a developer to sneak in subtle, obscure flaws.

Another problem here is that style formatting is a solved problem. Tools known as _code linters_ can solve all the problems that these certification reviews bring up. Modern software development tools can flag the developer as they go; if manufacturers are submitting code for review with these flaws, it means they haven't integrated these tools into their workflows. Given how easy they are to set up, this is a concern. It suggests that there is poor institutional control over source code.

#### The difference between Certification and Regulation

Generally speaking, certification programs are a quality assurance process that ensures manufacturers adhere to certain guidelines regarding design, safety, accuracy, fault tolerance, etc. Certifications can be used to satisfy regulations; however, legislatively-empowered regulations have more bite to them, and can carry civil and criminal penalties for non-compliance. Voting system regulations are delegated to the state level, and while election tampering remains a crime, certification programs are poorly equipped to catch deliberate design flaws.

#### Does my state regulate voting machines?

[This report](https://www.eac.gov/assets/1/Page/State%20Requirements%20and%20the%20Federal%20Voting%20System%20Testing%20and%20Certification%20Program.pdf) contains all you need to know, but I'll try to summarize things here.

| State | Reguations Required |
| ----- | ------------------- |
| AL    | Testing by Federally Accredited Laboratory |
| AK    | No requirements |
| AZ    | Testing by Federally Accredited Laboratory |
| AR    | No requirements |
| CA    | Federal Certification |
| CO    | Federal Certification |
| CT    | Testing to Federal Standards |
| DE    | Federal Certification |
| DC    | Testing to Federal Standards |
| FL    | No requirements |
| GA    | Federal Certification |
| HI    | No requirements |
| ID    | Federal Certification |
| IL    | Testing by Federally Accredited Laboratory |
| IN    | Testing to Federal Standards |
| IA    | Testing by Federally Accredited Laboratory |
| KS    | No requirements |
| KY    | Testing to Federal Standards |
| LA    | Testing by Federally Accredited Laboratory |
| ME    | No requirements |
| MD    | Testing by Federally Accredited Laboratory |
| MA    | Testing by Federally Accredited Laboratory |
| MI    | No requirements |
| MN    | Testing to Federal Standards |
| MS    | No requirements |
| MO    | Testing by Federally Accredited Laboratory |
| MT    | No requirements |
| NE    | No requirements |
| NV    | Testing to Federal Standards |
| NH    | No requirements |
| NJ    | No requirements |
| NM    | Testing by Federally Accredited Laboratory |
| NY    | Testing to Federal Standards |
| NC    | Federal Certification |
| ND    | Federal Certification |
| OH    | Federal Certification |
| OK    | No requirements |
| OR    | Testing to Federal Standards |
| PA    | Testing by Federally Accredited Laboratory |
| RI    | Testing by Federally Accredited Laboratory |
| SC    | Federal Certification |
| SD    | Federal Certification |
| TN    | No requirements |
| TX    | Testing to Federal Standards |
| UT    | Testing by Federally Accredited Laboratory |
| VT    | No requirements |
| VA    | Testing to Federal Standards |
| WA    | Federal Certification |
| WV    | No requirements |
| WI    | Testing by Federally Accredited Laboratory |
| WY    | Federal Certification |

#### About Accredited Laboratories

There are two pathways to becoming an accredited laboratory, according to [the Voting System Test Laboratories page](https://www.eac.gov/testing_and_certification/laboratory_accreditation.aspx) on the EAC website:

1. Fulfill NIST requirements pursuant to the Help America Vote Act of 2002 (42 USC §15371(b), Section 231(b));
2. Obtain a waiver from the EAC after explaining why you're so awesome.

According to [the NIST Voluntary Laboratory Accreditation Program database](https://www-s.nist.gov/niws/index.cfm?event=directory.search#no-back), there are three accredited Voting System Laboratories. (Under "Program" select "Voting System Testing", and under "Country" select "United States").

These are:

- SLI Compliance, formerly SLI Global Solutions
- National Technical Systems (NTS) Huntsville Operations, formerly Wyle Labs
- Pro V&V

In the summary of certified voting systems, another laboratory appears: iBeta Quality Assurance; however, it seems as though they stopped performing voting system testing somewhere around 2009.

## A Summary of the Software Review Process for all Certified Electronic Voting Systems

The comments here are not consistent per se; they are the nearest brief synopsis of the test procedure described in the Source Code Review section of the test reports.

This information is public and can be found on [the Testing and Certification Program page](https://www.eac.gov/testing_and_certification/default.aspx) of [the Election Assistance Commission's website](https://www.eac.gov/default.aspx). The Test Labs listed below use their current names, e.g. where the Certificate might say "Wyle Laboratories," this is now NTS Huntsville and is represented as such.

| Manufacturer | Device Name            | Standard  | Test Lab                | Certification Date | Software Review Comments                                        |
|--------------|------------------------|-----------|-------------------------|--------------------|-----------------------------------------------------------------|
| Dominion     | Democracy Suite 4.0    | VVSG 2005 | NTS Huntsville          | 10-May-12          | Style guide validation using Beyond Compare and Crimson Editor  |
| Dominion     | Democracy Suite 4.14   | VVSG 2005 | NTS Huntsville          | 7-Jul-13           | Style guide validation using Beyond Compare and Crimson Editor  |
| Dominion     | Democracy Suite 4.14a  | VVSG 2005 | NTS Huntsville          | 20-Sep-13          | Software testing deemed not necessary                           |
| Dominion     | Democracy Suite 4.14a1 | VVSG 2005 | NTS Huntsville          | 16-Jun-14          | Software compared to previous revision and verified unchanged   |
| Dominion     | Democracy Suite 4.14b  | VVSG 2005 | NTS Huntsville          | 7-Jan-14           | Style guide validation using Beyond Compare and Crimson Editor  |
| Dominion     | Democracy Suite 4.14d  | VVSG 2005 | NTS Huntsville          | 25-Nov-14          | Validated styles in 6 code modules                              |
| Dominion     | Democracy Suite 4.14e  | VVSG 2005 | NTS Huntsville          | 2-Jul-15           | Validated styles in 3 code modules                              |
| Dominion     | Assure 1.3             | VSS 2002  | SLI Compliance          | 29-Jun-12          | Reviewed 141 LOC for VVSG 2005 formatting, delta from previous rev |
| ES&S         | EVS 5.0.0.0            | VVSG 2005 | NTS Huntsville          | 16-May-13          | Style guide validation using Beyond Compare and Crimson Editor, manual visual scan, evaluation against mfg supplied coding standards |
| ES&S         | EVS 5.0.1.0            | VVSG 2005 | NTS Huntsville          | 18-Mar-14          | Style guide validation using Beyond Compare and Crimson Editor, manual visual scan, evaluation against mfg supplied coding standards" |
| ES&S         | EVS 5.2.0.0            | VVSG 2005 | NTS Huntsville          | 2-Jul-14           | Style guide validation using Beyond Compare and Crimson Editor, manual visual scan of non-java code, review of 10% of comments and headers, evaluation against mfg supplied coding standards | 
| ES&S         | EVS 5.2.0.3            | VVSG 2005 | NTS Huntsville          | 5-Aug-15           | Compared to ver 5.2.0.0 and evaluated against style guide       |
| ES&S         | EVS 5.2.0.4            | VVSG 2005 | NTS Huntsville          | 27-Apr-16          | Compared to ver 5.2.0.3 and evaluated against style guide, 291 LOC modified |
| ES&S         | EVS 5.2.1.0            | VVSG 2005 | NTS Huntsville          | 18-Dec-15          | Code from v 5.2.0.2 and 5.3.0.0-IL used for review | 
| ES&S         | EVS 5.2.1.1            | VVSG 2005 | NTS Huntsville          | 15-Jul-16          | Reviewed for adherence to style guide | 
| ES&S         | Unity 3.2.0.0          | VSS 2002  | NTS Huntsville          | 16-May-12          | Visual scan                                                     |
| ES&S         | Unity 3.2.1.0          | VSS 2002  | NTS Huntsville          | 29-Mar-11          | Functionality and regression tests following anomaly            |
| ES&S         | Unity 3.4.0.0          | VSS 2002  | NTS Huntsville          | 31-Oct-12          | Visual scan                                                     |
| ES&S         | Unity 3.4.1.0          | VSS 2002  | NTS Huntsville          | 4-Apr-14           | Style guide validation using Beyond Compare and Crimson Editor, manual visual scan, evaluation against mfg supplied coding standards |
| ES&S         | Unity 3.4.1.4          | VVSG 2005 | NTS Huntsville          | 26-Aug-16          | Reviewed 1552 LOC for style adherence                           |
| ES&S         | Assure 1.2             | VSS 2002  | iBeta Quality Assurance | 6-Aug-09           | Reviewed 3% of code, focused on functions that handle vote data, audits, reporting |
| Hart         | Verity 1.0             | VVSG 2005 | SLI Compliance          | 12-May-15          | Verified modularity, integrity, and style |
| Hart         | Verity 2.0             | VVSG 2005 | SLI Compliance          | 27-Apr-16          | Verified modularity, integrity, and style |
| MicroVote    | EMS 4.0                | VVSG 2005 | iBeta Quality Assurance | 6-Feb-09           | Verified for standards compliance |
| MicroVote    | EMS 4.0b               | VVSG 2005 | NTS Huntsville          | 23-Aug-10          | Verified for standards compliance |
| MicroVote    | EMS 4.1                | VVSG 2005 | NTS Huntsville          | 16-Jul-15          | Compared to 4.0B baseline and reviewed for adherence |
| Unisyn       | OpenElec 1.0           | VVSG 2005 | NTS Huntsville          | 13-Jan-10          | Adherence review, peer review of 10% of codebase |
| Unisyn       | OpenElec 1.0.1         | VVSG 2005 | NTS Huntsville          | 21-Jul-11          | Compared to 1.0 baseline, visual review of changes |
| Unisyn       | OpenElec 1.1           | VVSG 2005 | NTS Huntsville          | 9-Apr-12           | Visual scan of every line for compliance |
| Unisyn       | OpenElec 1.2           | VVSG 2005 | NTS Huntsville          | 23-Dec-13          | "Validation using Eclipse and Checkstyle, 10% manual review of headers and comments |
| Unisyn       | OpenElec 1.3           | VVSG 2005 | NTS Huntsville          | 12-Jan-15          | "Validation using Eclipse and Checkstyle, 10% manual review of headers and comments |
