Capitains Guidelines Draft 2.3
===

*This version of the guidelines is a draft. This will import new ways to express collections metadata and citation systems without depending on CTS. They will not replace older guidelines but we would highly recommend to migrate to these new ones.*

# Metadata

# Texts

We are reducing the amount of nodes that are used by Capitains. We are also building this new system in order to allow multiple citation system.

The only required set of nodes in Capitains 2.3 guidelines is a refsDecl with `n="capitains"` and `xml:base` set with the ID of your text. this will allow in the future for multi identifier/citation system.
 
 The previous system of cRefPattern is unchanged except :
1. order does not matter anymore : the level of the refsDecl is declared in `@corresp`.
2. the `matchPattern` will be compiled as regex and will matter, unlike before. 
	- They must be unique.
 	- Dot can't match it all.
 	- The number of dot should be equal to the level - 1 : level two muse have at least one dot.
 
 The second point means that ultimately, you could have different node matching system depending on some things, eg. l1 could match a line (`(l\d+)`) while p1 a paragraph (`(p\d+)`). 
 
 The use of multiple match pattern for the same level will mean that it will be most likely less performant but we need to make it possible.
 
 Example :
 
 ```xml
<refsDecl n="capitains" xml:base="urn:cts:latinLit:phi1294.phi002.perseus-lat2">
<cRefPattern corresp="3" n="line"
             matchPattern="(\w+).(\w+).(\w+)"
             replacementPattern="#xpath(/tei:TEI/tei:text/tei:body/tei:div/tei:div[@n='$1']/tei:div[@n='$2']/tei:l[@n='$3'])">
    <p>This pointer pattern extracts book and poem and line</p>
</cRefPattern>
<cRefPattern corresp="2" n="poem"
             matchPattern="(\w+).(\w+)"
             replacementPattern="#xpath(/tei:TEI/tei:text/tei:body/tei:div/tei:div[@n='$1']/tei:div[@n='$2'])">
    <p>This pointer pattern extracts book and poem</p>
</cRefPattern>
<cRefPattern corresp="1" n="book"
             matchPattern="(\w+)"
             replacementPattern="#xpath(/tei:TEI/tei:text/tei:body/tei:div/tei:div[@n='$1'])">
    <p>This pointer pattern extracts book</p>
</cRefPattern>
</refsDecl>
 ```