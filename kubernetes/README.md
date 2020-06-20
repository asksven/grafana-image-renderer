# Deploy the grafana-image-renderer to Kubernetes

The assumption is that you have Grafana already running on Kubernetes, deployed with the [official helm chart](https://github.com/helm/charts/tree/master/stable/grafana)

## Deployment

1. Change the path to your docker images in `deployment.yaml`, `spec.template.spec.containers.image`
1. Assuming that your Grafana instance is deployed in the `grafana` namespace, deploy: `kubectl -n grafana apply -f *.yaml`

## Changes to the chart

In order to delegate image rendering to the remote image renderer these changes are required to the environment variables in the [`values.yaml`](https://github.com/helm/charts/blob/master/stable/grafana/values.yaml):

```
env: 
  GF_RENDERING_SERVER_URL: http://grafana-image-renderer:8081/render
  GF_RENDERING_CALLBACK_URL: http://grafana:80/
  GF_LOG_FILTERS: rendering:debug 
```

Apply the chart and you are good to go.

## Testing

Open any panel in Grafana, select the contect-menu from a panel, select "Share", and click on "Direct link rendered image": the rendered image should appear.

 

