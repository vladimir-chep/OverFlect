<script>
import { Line } from 'vue-chartjs';

export default {
    extends: Line,
    props: {
        chartData: {
            type: Object,
            required: true,
        },
    },
    data() {
        return {
            options: {
                responsive: true,
                maintainAspectRatio: false,
            },
            gradient: null,
            htmlLegend: null,
        };
    },
    computed: {
        labels: function() {
            return this.chartData.order;
        },
        score: function() {
            return this.chartData.score;
        },
    },
    mounted() {
        this.renderLineChart();
    },
    methods: {
        renderLineChart: function() {
            this.gradient = this.$refs.canvas
                .getContext('2d')
                .createLinearGradient(0, 0, 0, 450);
            this.gradient.addColorStop(0, 'rgba(123, 75, 255, 0.5)');
            this.gradient.addColorStop(0.5, 'rgba(123, 75, 255, 0.25)');
            this.gradient.addColorStop(1, 'rgba(123, 75, 255, 0)');
            this.renderChart(
                {
                    labels: this.labels,
                    datasets: [
                        {
                            label: 'Score',
                            borderColor: '#7b4bff',
                            backgroundColor: this.gradient,
                            pointBackgroundColor: '#7b4bff',
                            borderWidth: 2,
                            pointBorderColor: 'white',
                            data: this.score,
                        },
                    ],
                },
                this.options
            );
        },
    },
    watch: {
        chartData() {
            if (this.$data._chart) {
                this.$data._chart.destroy();
            }
            this.renderLineChart();
        },
    },
};
</script>
