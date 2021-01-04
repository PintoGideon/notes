```javascript

<div className="Timeline" ref={ref}>
<svg className="Chart">
</svg>

</div>

```


### Data Hierarchy

```javascript

<div className="canvas">


</div>


const stratify=d3.stratify()
.id(d=>d.name)
.parentId(d=>d.parent)

stratify(data)
//

```

```javascript

<div class="container">
<div class="canvas"></div>
</div>

```

```css
.canvas{
margin-top:40px
}
```

```javascript

const dimensions={height:500, width:1100}
const svg=d3.select('.canvas').append('svg')
.attr('width',dims.width+100)
.attr('height',dims.height+100)

const graph=svg.append('g')
.attr('transform','translate(50,50)')

// Data Strat

const stratify=d3.stratify().id(d=>d.name)
               .parentId(d=>d.parent)

const tree=d3.tree(),size([
    dims.width,dims.height
])




//update function

const update=(data)=>{

// get updated root node data
const rootNode=stratify(data)
const treeData=tree(rootNode);

// get nodes selection and join data

const nodes=graph.selectAll('.node')
.data(treeData,descendants())

// Create Enter node groups

const enterNodes=nodes.enter()
.append('g')
.attr('class','node')
.attr('transform',(d)=>{
`translate(${d.x},${d.y})`
})


const links=graph.selectAll('.link')
.data(treeData.links())

// Enter new links

links.enter().append('path')
.attr('class','link')
.attr('fill','none')
.attr('stroke','#aaa')
.attr('stroke-width',2)
.attr('d',d3.linkVertical().x(d=>d.x)
.y(d=>d.y))






enterNodes.append('rect')
.attr('fill','#aaa')
.attr('stroke','#555')
.attr('stroke-width',2)
,attr('r','8')

enterNodes.append('text')
.attr('text-anchor',middle)
.attr('fill','white')
.text(d=>d.name)


graph.selectAll('.node').remove()
graph.selectAll('.link').remove()


const color=d3.scaleOrdinal(['white','black','red'])

color.domain(data.map(item)=>item.department)

}


```


