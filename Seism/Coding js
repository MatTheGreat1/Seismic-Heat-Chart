document.getElementById('fileInput').addEventListener('change', function(event) {
    const file = event.target.files[0];
    if (file) {
        const reader = new FileReader();
        reader.onload = function(e) {
            const data = e.target.result;
            renderHeatmap(data);
        };
        reader.readAsText(file);
    }
});

function renderHeatmap(data) {
    // Split the data into rows and then into columns
    const rows = data.trim().split("\n").map(row => row.split(",").map(Number));

    // Set dimensions and margins for the graph
    const margin = {top: 30, right: 30, bottom: 30, left: 30},
          width = 450 - margin.left - margin.right,
          height = 450 - margin.top - margin.bottom;

    // Create an SVG element
    const svg = d3.select("#my_dataviz")
        .append("svg")
        .attr("width", width + margin.left + margin.right)
        .attr("height", height + margin.top + margin.bottom)
        .append("g")
        .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

    // Define groups and variables based on the data
    const myGroups = d3.range(rows[0].length); // Number of columns
    const myVars = d3.range(rows.length); // Number of rows

    // Build X scale and axis
    const x = d3.scaleBand()
        .range([0, width])
        .domain(myGroups)
        .padding(0.01);
    
    svg.append("g")
        .attr("transform", "translate(0," + height + ")")
        .call(d3.axisBottom(x).tickFormat(d => `Col ${d + 1}`)); // Custom tick format

    // Build Y scale and axis
    const y = d3.scaleBand()
        .range([height, 0])
        .domain(myVars)
        .padding(0.01);
    
    svg.append("g")
        .call(d3.axisLeft(y).tickFormat(d => `Row ${d + 1}`)); // Custom tick format

    // Build color scale with slightly darker colors
    const myColor = d3.scaleLinear()
        .range(["#e0e0e0", "#004d4d"]) // Adjusted for slightly darker shades
        .domain([d3.min(rows.flat()), d3.max(rows.flat())]); // Color scale based on data range

    // Create the heatmap cells with increased thickness
    const boxThicknessFactor = 1.2; // Factor to increase the box thickness
    svg.selectAll()
        .data(rows.flat())
        .enter()
        .append("rect")
        .attr("x", (d, i) => x(i % myGroups.length))
        .attr("y", (d, i) => y(Math.floor(i / myGroups.length)))
        .attr("width", x.bandwidth() * boxThicknessFactor) // Increased width
        .attr("height", y.bandwidth() * boxThicknessFactor) // Increased height
        .style("fill", d => myColor(d));
}
