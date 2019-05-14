# JSXN - JSON representation of XML by Proxy

Currently experimental, expect updates to break things

## Example usage
```javascript
const parser = require('slimdom-sax-parser')
import {XMLDocument, Element} from "slimdom"
import {makeXmlProxy, Rule} from "./index"

const sampleXml = `
<root xmlns:y="http://localhost/yellow" xmlns:g="http://localhost/green">
    <tree>
        <branch>br1</branch>
        <branch>br2</branch>
    </tree>
    <plant type="shrub"/>
    <plant type="bush"/>
    <option value="true">
        <value>false</value>
    </option>
    <y:yellow a="b">
        text
    </y:yellow>
    <g:yellow>
        green
    </g:yellow>
</root>
`
const document:XMLDocument = parser.sync(sampleXml)

const rules:Rule[] = [
    {key:'plant', type: 'elements'},
    {key:'value', type: 'attribute'},
    {key:'green', asKey: 'yellow', asNamespace: 'http://localhost/green'},
    {key:'v', asKey: 'type', whenLocalName: 'plant'},
    {key:'v', asKey: 'value'},
]
const jsxn:any = makeXmlProxy(document.documentElement as Element, rules)

console.log(jsxn.tree.branch)
console.log(jsxn.plant[1].type)
console.log(jsxn.option.value)
console.log(jsxn.option.v)
console.log(jsxn.plant[0].v)
console.log(jsxn.yellow.a)
console.log(jsxn.green)
```