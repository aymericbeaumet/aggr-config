---
title: Research Paper Release Checklist
link: https://nicholas.carlini.com/writing/2022/paper-release-checklist.html
source: nicholas-carlini-com
published: 2022-01-30T00:00:00Z
updated: 2022-01-30T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: This page contains a few checklists that help prevent embarrassing issues when releasing research papers online (e.g., via arXiv or a conference publication).
content: extracted
html: 2022-01-30-research-paper-release-checklist.html
---

Over the years I've made a number of simple, embarrassing mistakes when putting papers online. Three of the worst offenders here are that time I put a paper online with [a typo in the title](https://arxiv.org/pdf/2202.07646v1.pdf), published a paper with [my own name spelled wrong (pages 1 & 2)](https://www.usenix.org/system/files/conference/usenixsecurity15/sec15-paper-carlini.pdf), and that other time I released a paper with [a git merge conflict right in the middle of the text (page 24)](https://arxiv.org/pdf/2002.08347v1.pdf).

Fortunately there's an unreasonably effective method to preventing these mistakes. Every time I make one (or someone tells me a mistake they made that I could easily see myself making) I write it down and add it to a checklist. Each paper I release, I run through the checklist, and as a result will only ever make the same mistake once.

(This is not my idea. Checklists are the primary reason airplanes don't fall out of the sky, and help prevent an uncountable number of deaths in hospitals every day. They're especially useful in boring situations, where you start to relax because the hard part is over. Like, for example, when doing the final upload for a paper after having just spent several months putting it together.)

I recently mentioned that I do this to some people who asked me to share it, and realized this is something that others might find useful as well. So here it is. Incidentally, if you've ever made an easily-preventable mistake, feel free to let me know and I'll update this list.

* * *

## Primary checklist for any paper release

This checklist is for any time a paper is being released Follow it any time you're submitting to a conference, uploading to arXiv, or submitting a camera ready.

1.  Check the paper title is correct.
2.  Completely disable any author TODO notes LaTeX macros.
3.  Check LaTeX build log and search for "error", "warning", "undefined", "missing".
4.  Remove any LaTeX macros from abstract for upload to remote server.
5.  Search for doubled words:

    ```
    r=open("/tmp/paper.tex").read().split();
    ```

    ```
    [a for a,b in zip(r,r[1:]) if a.lower() == b.lower()]
    ```

6.  Inspect any lines that have a question mark or double periods:

    ```
    [x for x in open("/tmp/paper.tex") if '..' in x or '?' in x]
    ```

7.  Make sure the word TODO and FIXME are never present:

    ```
     [x for x in open("/tmp/paper.tex") if 'TODO' in x or 'FIXME' in x]
    ```

8.  Verify there are no merge conflicts by looking for '=====', '<<<<', '>>>>'.
9.  Visually check that no paragraphs end with a single word on a new line, and no single lines occur at the top of a new page right below a caption. Both of these are ugly.
10. Re-read all captions to make sure they are understandable in near-isolation, and that any final edits didn't break these.
11. Print out PDF from another computer, check fonts still render, and figures are legible.

## Secondary checklist for conference submissions

This checklist is a specific checklist when submitting a paper to a conference. Do the above checklist, and also this one.

1. Check call for papers to ensure rules about blind/non-blind submission. If blind, search for all author names to ensure they are not present in the document.
2. Follow the CFP rules if pages should be numbered or not numbered.
3. Count pages and verify it's no longer than the page limit.
4. Check for any special rules on the total number of pages (including the appendix and references) versus content pages.
5. Check bibliography style matches what's expected for this conference.
6. For security papers include something like, 'anonymous submission #xyz' as the author name (with a random formatting, especially when submitting multiple papers to the same conference, to not break anonymity).

## Secondary checklist for public releases

This checklist is a specific checklist any time a paper is being released publicly, e.g., as a camera-ready final paper or an arXiv upload. Do the primary checklist and this one.

1. All authors are listed on the paper.
2. Author names are spelled correctly, with correct affiliations.
3. Change the template to the “accepted” version of the style, if applicable.
4. Verify including author names doesn't break the flow of text on the page, especially page 1.
5. Check that any copyright block of publication notice is added.
6. Check for the presence of Type 3 fonts, and if they're in the paper, and the conference cares, then I guess just give up and admit you're going to spend the next six hours figuring out how to fix it this time around.
7. Un-comment the acknowledgments section, and any author contribution section.

## Secondary checklist for arXiv paper releases

This checklist is a specific checklist any time a paper is being released to arXiv. You should also follow the public release checklist above.

1. Decide if it's worth running arXiv cleaner to remove comments or if it's okay to others see paper comments.
2. Check that author names on the upload match author names on the submission (except for authors who prefer slight variants on their name for arXiv, e.g., by replacing non-English characters with their English counterpart).
3. Check that any copyright block of publication notice is REMOVED, as appropriate for the venue.
4. Check abstract is formatted correctly without any LaTeX macros present.
5. View rendered PDF and check that formatting is consistent with a local copy.
