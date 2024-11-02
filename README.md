<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Speed and RPM Gauge</title>
    <style>
        .highcharts-figure .chart-container {
            width: 300px;
            height: 200px;
            float: left;
        }
        .highcharts-figure {
            width: 600px;
            margin: 0 auto;
        }
        .highcharts-description {
            text-align: center;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <script src="https://code.highcharts.com/highcharts.js"></script>
    <script src="https://code.highcharts.com/highcharts-more.js"></script>
    <script src="https://code.highcharts.com/modules/solid-gauge.js"></script>

    <figure class="highcharts-figure">
        <div id="container-speed" class="chart-container"></div>
        <div id="container-rpm" class="chart-container"></div>
        <p class="highcharts-description">Speed and RPM Dashboard</p>
    </figure>

    <script>
        // Common gauge options
        const gaugeOptions = {
            chart: {
                type: 'solidgauge'
            },
            title: null,
            pane: {
                center: ['50%', '85%'],
                size: '140%',
                startAngle: -90,
                endAngle: 90,
                background: {
                    backgroundColor: '#f0f0f0',
                    innerRadius: '60%',
                    outerRadius: '100%',
                    shape: 'arc'
                }
            },
            yAxis: {
                stops: [
                    [0.1, '#55BF3B'], // green
                    [0.5, '#DDDF0D'], // yellow
                    [0.9, '#DF5353']  // red
                ],
                lineWidth: 0,
                tickWidth: 0,
                minorTickInterval: null,
                tickAmount: 2,
                title: {
                    y: -70
                },
                labels: {
                    y: 16
                }
            },
            plotOptions: {
                solidgauge: {
                    dataLabels: {
                        y: 5,
                        borderWidth: 0,
                        useHTML: true
                    }
                }
            }
        };

        // Speed gauge
        const chartSpeed = Highcharts.chart('container-speed', Highcharts.merge(gaugeOptions, {
            yAxis: {
                min: 0,
                max: 200,
                title: {
                    text: 'Speed'
                }
            },
            series: [{
                name: 'Speed',
                data: [80],
                dataLabels: {
                    format:
                        '<div style="text-align:center">' +
                        '<span style="font-size:25px">{y}</span><br/>' +
                        '<span style="font-size:12px;opacity:0.4">km/h</span>' +
                        '</div>'
                },
                tooltip: {
                    valueSuffix: ' km/h'
                }
            }]
        }));

        // RPM gauge
        const chartRpm = Highcharts.chart('container-rpm', Highcharts.merge(gaugeOptions, {
            yAxis: {
                min: 0,
                max: 5,
                title: {
                    text: 'RPM'
                }
            },
            series: [{
                name: 'RPM',
                data: [1],
                dataLabels: {
                    format:
                        '<div style="text-align:center">' +
                        '<span style="font-size:25px">{y:.1f}</span><br/>' +
                        '<span style="font-size:12px;opacity:0.4">* 1000 / min</span>' +
                        '</div>'
                },
                tooltip: {
                    valueSuffix: ' revolutions/min'
                }
            }]
        }));

        // Update gauges periodically
        setInterval(function () {
            // Update Speed
            if (chartSpeed) {
                const point = chartSpeed.series[0].points[0];
                const newVal = Math.floor(Math.random() * 201); // Random speed value between 0 and 200
                point.update(newVal);
            }

            // Update RPM
            if (chartRpm) {
                const point = chartRpm.series[0].points[0];
                const newVal = (Math.random() * 5).toFixed(1); // Random RPM value between 0 and 5
                point.update(parseFloat(newVal));
            }
        }, 2000); // Update every 2 seconds
    </script>
</body>
</html>
