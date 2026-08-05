<script lang="ts">
  import { onMount } from 'svelte'
  import * as d3 from 'd3'

  // Total CVEs per year — exact bar labels from the source chart.
  // KEV counts are read off the secondary (0–500) axis of the same chart and are
  // approximate; used only to show the "exploited-in-the-wild leveled off" shape.
  // Source: "Vulnerabilities Continue to Rocket Higher in 2025", Rob Lemos / Dark Reading.
  // Data from the National Vulnerability Database and CISA's KEV Catalog.
  const data = [
    { year: 2006, cves: 6608, kev: 5 },
    { year: 2007, cves: 6516, kev: 6 },
    { year: 2008, cves: 5632, kev: 9 },
    { year: 2009, cves: 5732, kev: 13 },
    { year: 2010, cves: 4639, kev: 22 },
    { year: 2011, cves: 4150, kev: 10 },
    { year: 2012, cves: 5288, kev: 22 },
    { year: 2013, cves: 5187, kev: 35 },
    { year: 2014, cves: 7928, kev: 33 },
    { year: 2015, cves: 6494, kev: 42 },
    { year: 2016, cves: 6449, kev: 52 },
    { year: 2017, cves: 14642, kev: 88 },
    { year: 2018, cves: 16510, kev: 75 },
    { year: 2019, cves: 17305, kev: 127 },
    { year: 2020, cves: 18322, kev: 145 },
    { year: 2021, cves: 20144, kev: 208 },
    { year: 2022, cves: 25074, kev: 128 },
    { year: 2023, cves: 28817, kev: 160 },
    { year: 2024, cves: 39965, kev: 152 },
    { year: 2025, cves: 48177, kev: 165 },
  ]

  let svgEl: SVGSVGElement

  onMount(() => {
    const margin = { top: 24, right: 52, bottom: 44, left: 52 }
    const width = 760 - margin.left - margin.right
    const height = 400 - margin.top - margin.bottom

    const svg = d3.select(svgEl)
      .attr('viewBox', `0 0 ${width + margin.left + margin.right} ${height + margin.top + margin.bottom}`)

    const g = svg.append('g')
      .attr('transform', `translate(${margin.left},${margin.top})`)

    const x = d3.scaleBand<number>()
      .domain(data.map(d => d.year))
      .range([0, width])
      .padding(0.28)

    const y = d3.scaleLinear()
      .domain([0, 50000])
      .range([height, 0])

    const yKev = d3.scaleLinear()
      .domain([0, 500])
      .range([height, 0])

    // Grid lines
    g.append('g')
      .call(d3.axisLeft(y).ticks(5).tickSize(-width).tickFormat(() => ''))
      .selectAll('line')
      .attr('stroke', 'rgba(255,255,255,0.06)')
    g.selectAll('.domain').remove()

    // Bar color: blue (older) → pink (recent) to convey acceleration
    const color = d3.scaleSequential(d3.interpolateRgb('#3b82f6', '#ec4899'))
      .domain([2006, 2025])

    // Clip rect for left-to-right reveal
    const clipId = 'vuln-clip-' + Math.random().toString(36).slice(2, 8)
    const clipRect = svg.append('defs').append('clipPath').attr('id', clipId)
      .append('rect')
      .attr('x', 0).attr('y', -margin.top)
      .attr('width', 0).attr('height', height + margin.top + margin.bottom)

    const content = g.append('g').attr('clip-path', `url(#${clipId})`)

    // Bars
    content.selectAll('rect.bar')
      .data(data)
      .enter().append('rect')
      .attr('class', 'bar')
      .attr('x', d => x(d.year)!)
      .attr('y', d => y(d.cves))
      .attr('width', x.bandwidth())
      .attr('height', d => height - y(d.cves))
      .attr('rx', 2)
      .attr('fill', d => color(d.year))

    // Label the final bar
    const last = data[data.length - 1]
    content.append('text')
      .attr('x', x(last.year)! + x.bandwidth() / 2)
      .attr('y', y(last.cves) - 8)
      .attr('text-anchor', 'middle')
      .attr('fill', '#ec4899')
      .attr('font-size', '13px')
      .attr('font-weight', '700')
      .attr('font-family', 'Mona Sans, sans-serif')
      .text(last.cves.toLocaleString())

    // KEV line (secondary axis)
    const line = d3.line<typeof data[0]>()
      .x(d => x(d.year)! + x.bandwidth() / 2)
      .y(d => yKev(d.kev))
      .curve(d3.curveMonotoneX)

    content.append('path')
      .datum(data)
      .attr('fill', 'none')
      .attr('stroke', '#f8444d')
      .attr('stroke-width', 2.5)
      .attr('d', line)

    content.selectAll('circle.kev')
      .data(data)
      .enter().append('circle')
      .attr('class', 'kev')
      .attr('cx', d => x(d.year)! + x.bandwidth() / 2)
      .attr('cy', d => yKev(d.kev))
      .attr('r', 3)
      .attr('fill', '#f8444d')

    // Reveal animation
    function playAnimation() {
      clipRect.interrupt()
      clipRect.attr('width', 0)
        .transition()
        .duration(2200)
        .ease(d3.easeLinear)
        .attr('width', width + 4)
    }

    const section = svgEl.closest('section')
    if (section) {
      const observer = new MutationObserver(() => {
        if (section.classList.contains('present')) playAnimation()
      })
      observer.observe(section, { attributes: true, attributeFilter: ['class'] })
      if (section.classList.contains('present')) playAnimation()
    } else {
      playAnimation()
    }

    // X axis (every other year to avoid crowding)
    g.append('g')
      .attr('transform', `translate(0,${height})`)
      .call(d3.axisBottom(x).tickValues(data.filter((_, i) => i % 2 === 0).map(d => d.year)).tickFormat(d3.format('d')))
      .selectAll('text')
      .attr('fill', 'rgba(255,255,255,0.5)')
      .attr('font-size', '10px')
      .attr('font-family', 'IBM Plex Mono, monospace')
    g.selectAll('.tick line').attr('stroke', 'rgba(255,255,255,0.1)')
    g.selectAll('.domain').attr('stroke', 'rgba(255,255,255,0.15)')

    // Y axis left (Total CVEs)
    g.append('g')
      .call(d3.axisLeft(y).ticks(5).tickFormat(d => `${(d as number) / 1000}K`))
      .selectAll('text')
      .attr('fill', 'rgba(255,255,255,0.5)')
      .attr('font-size', '10px')
      .attr('font-family', 'IBM Plex Mono, monospace')
    g.selectAll('.domain').remove()

    // Y axis right (KEV)
    g.append('g')
      .attr('transform', `translate(${width},0)`)
      .call(d3.axisRight(yKev).ticks(5))
      .selectAll('text')
      .attr('fill', 'rgba(248,68,77,0.7)')
      .attr('font-size', '10px')
      .attr('font-family', 'IBM Plex Mono, monospace')
    g.selectAll('.domain').remove()
  })
</script>

<div class="chart-wrap">
  <svg bind:this={svgEl} class="chart-svg"></svg>
  <div class="legend">
    <span class="key"><span class="swatch bar"></span>Total CVEs</span>
    <span class="key"><span class="swatch line"></span>Known Exploited (KEV)</span>
  </div>
  <p class="source">Data: NVD &amp; CISA KEV Catalog · chart after Rob Lemos / Dark Reading</p>
</div>

<style>
  .chart-wrap {
    max-width: 820px;
    margin: 0 auto;
    text-align: center;
  }

  .chart-svg {
    width: 100%;
    height: auto;
  }

  .legend {
    display: flex;
    justify-content: center;
    gap: 1.5rem;
    margin-top: 0.5rem;
  }

  .key {
    display: inline-flex;
    align-items: center;
    gap: 0.4rem;
    font-size: 0.8rem;
    color: rgba(255, 255, 255, 0.6);
    font-family: "IBM Plex Mono", monospace;
  }

  .swatch {
    display: inline-block;
    width: 14px;
    height: 10px;
    border-radius: 2px;
  }

  .swatch.bar {
    background: linear-gradient(90deg, #3b82f6, #ec4899);
  }

  .swatch.line {
    height: 3px;
    background: #f8444d;
  }

  .source {
    font-size: 0.6rem;
    color: rgba(255, 255, 255, 0.3);
    margin-top: 0.4rem;
    font-family: "IBM Plex Mono", monospace;
  }
</style>
