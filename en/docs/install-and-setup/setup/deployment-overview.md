# WSO2 API Manager Deployment Overview

WSO2 API Manager consists of an API management layer and an integration layer. The API management layer contains several components, which you can use in your deployment according to your requirement. The integration layer includes either the Micro Integrator runtime (for services integration) and the Streaming Integrator runtime (for streaming requirements) or both runtimes.

You can select one of the following deployment patterns depending on the workload of each component and the traffic that is expected to each of the components and runtimes.

!!! note
    From WSO2 API Manager 4.5.0 onwards, we no longer support API-M profiles such as, `-Dprofile=control-plane`, `-Dprofile=gateway-worker` and `-Dprofile=traffic-manager`. Instead, we now have separate distributions namely, **WSO2 API Control Plane**, **WSO2 Universal Gateway** and **WSO2 Traffic Manager** components which should be used for configuring a distributed deployment. For alternative deployment options, please refer the table below. See more information in [Key Changes]({{base_path}}/get-started/about-this-release/#key-changes).

    ??? info "New Deployment Alternatives"

        <table>
            <colgroup>
                <col />
                <col />
                <col />
                <col />
                <col />
                <col />
            </colgroup>
            <thead>
                <tr>
                    <th>Previous Deployment Pattern</th>
                    <th colspan="5">New Deployment Alternative</th>
                </tr>
                <tr>
                    <th></th>
                    <th>All-in-one</th>
                    <th>API Control Plane (ACP)</th>
                    <th>Universal Gateway</th>
                    <th>Traffic Manager</th>
                    <th>Key Manager of ACP</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td>All-in-one</td>
                    <td style="text-align: center;">&#x2713;</td>
                    <td></td>
                    <td></td>
                    <td></td>
                    <td></td>
                </tr>
                <tr>
                    <td>Control Plane + Gateway</td>
                    <td style="text-align: center;">&#x2713;</td>
                    <td></td>
                    <td style="text-align: center;">&#x2713;</td>
                    <td></td>
                    <td></td>
                </tr>
                <tr>
                    <td>Control Plane + Gateway + Traffic Manager</td>
                    <td></td>
                    <td style="text-align: center;">&#x2713;</td>
                    <td style="text-align: center;">&#x2713;</td>
                    <td style="text-align: center;">&#x2713;</td>
                    <td></td>
                </tr>
                <tr>
                    <td>Control Plane + Gateway + Key Manager</td>
                    <td style="text-align: center;">&#x2713;</td>
                    <td></td>
                    <td style="text-align: center;">&#x2713;</td>
                    <td></td>
                    <td style="text-align: center;">&#x2713;</td>
                </tr>
                <tr>
                    <td>Control Plane + Gateway + Traffic Manager + Key Manager</td>
                    <td></td>
                    <td style="text-align: center;">&#x2713;</td>
                    <td style="text-align: center;">&#x2713;</td>
                    <td style="text-align: center;">&#x2713;</td>
                    <td style="text-align: center;">&#x2713;</td>
                </tr>
            </tbody>
        </table>

## Standard HA deployment

This deployment consists of an API-M cluster with two nodes of the API-M runtime and two nodes each of the integration runtimes (Micro Integrator/Streaming Integrator). You can use this pattern if you expect to receive low traffic to your deployment. 

!!! Note 
    Two nodes of each component is used to ensure minimum high availability in all components.

<a href="{{base_path}}/assets/img/setup-and-install/basic-ha-deployment.png"><img src="{{base_path}}/assets/img/setup-and-install/basic-ha-deployment.png" alt="standard HA deployment" width="50%"></a>

### API-M cluster

The API-M cluster consists of two <b>All-in-One</b> API-M nodes. See the following link for instructions on how to set up this cluster.

<ul>
    <li>
        <a href="{{base_path}}/install-and-setup/setup/single-node/configuring-an-active-active-deployment">API-M Cluster with Minimum High Availability</a>
    </li>
</ul>

### Integration clusters

The integration cluster may be a Micro Integrator cluster or a Streaming Integrator cluster or two clusters of each. See the following links for instructions on how to set up this cluster.

<ul>
    <li>
        <a href="https://mi.docs.wso2.com/en/4.3.0/install-and-setup/setup/deployment/deploying-wso2-mi/">Micro Integrator Cluster with Minimum High Availability</a>
    </li>
    <li>
        <a href="https://ei.docs.wso2.com/en/latest/streaming-integrator/setup/deploying-si-as-minimum-ha-cluster/">Streaming Integrator Cluster with Minimum High Availability</a>
    </li>
</ul>

## Standard HA deployment with multitenancy

This deployment consists of two API-M nodes and two nodes each of the integration runtimes (Micro Integrator/Streaming Integrator) **per tenant**. You can use this pattern when traffic from different tenants in the API-M cluster needs to be handled in isolation. This deployment also allows you to direct the traffic of each tenant to a separate integration cluster. 

Although API-M nodes are capable of handling in-jvm multitenancy, Micro Integrator/Streaming Integrator nodes are not. Therefore, to handle traffic to different tenants, you need to set up different clusters of the integration runtimes and configure traffic routing accordingly. 

!!! Note
    The basic deployment suggests two nodes of each runtime to ensure minimum high availability. However, you can independently scale them depending on the resource requirements for each tenant.

<a href="{{base_path}}/assets/img/setup-and-install/basic-ha-with-multitenancy.png"><img src="{{base_path}}/assets/img/setup-and-install/basic-ha-with-multitenancy.png" alt="standard HA with multitenancy" width="80%"></a>

### API-M cluster

The API-M cluster consists of two <b>All-in-One</b> API-M nodes. See the following link for instructions on how to set up this cluster.

<ul>
    <li>
        <a href="{{base_path}}/install-and-setup/setup/single-node/configuring-an-active-active-deployment">API-M Cluster with Minimum High Availability</a>
    </li>
</ul>

### Integration cluster

The integration cluster consists of two nodes of the integration runtime for each of the tenants in the API-M cluster. See the following links for instructions on how to set up this cluster.

<ul>
    <li>
        <a href="https://mi.docs.wso2.com/en/4.3.0/install-and-setup/setup/deployment/deploying-wso2-mi/">Micro Integrator Cluster with Minimum High Availability</a>
    </li>
    <li>
        <a href="https://ei.docs.wso2.com/en/latest/streaming-integrator/setup/deploying-si-as-minimum-ha-cluster/">Streaming Integrator Cluster with Minimum High Availability</a>
    </li>
</ul>

## Simple scalable deployment

This pattern allows you to scale the deployment on demand. The **simple scalable deployment** pattern shown below illustrates a deployment that uses minimum resources. However, this setup can easily be scaled.

You need to set up four clusters of the different components and runtimes as they have different scaling requirements.

!!! Note
    The basic deployment suggests two nodes of each runtime to ensure minimum high availability. However, you can independently scale them depending on the requirements.

<a href="{{base_path}}/assets/img/setup-and-install/deployment-pattern4.png"><img src="{{base_path}}/assets/img/setup-and-install/deployment-pattern4.png" alt="Simple scalability" width="80%"></a>

### API-M cluster

The API-M layer of this deployment consists of three clusters of API-M component distributions as follows:

<table>
    <tr>
        <th>
            Control Plane Cluster
        </th>
        <td>
            The Control Plane cluster consists of two nodes of the <b>API Control Pane</b> distribution (Publisher, Devportal, Key Manager). The two node cluster is the simplest deployment for this pattern. If required you can scale the number of nodes.
        </td>
    </tr>
    <tr>
        <th>
            Data Plane Cluster
        </th>
        <td>
            The <b>Universal Gateway</b> distribution of API-M is deployed as a separate cluster so that we can scale it to match the traffic requirements. The simplest deployment for this pattern consists of a two node cluster. If required you can scale the number of nodes.
        </td>
    </tr>
    <tr>
        <th>
            Traffic Manager Cluster
        </th>
        <td>
            The <b>Traffic Manager</b> distribution of API-M is deployed as a separate cluster so that we can scale it to address requirements of rate limiting of requests processed by the data plane. The simplest deployment for this pattern consists of a two node cluster. If required you can scale the number of nodes.
        </td>
    </tr>
</table>

To set up this cluster, see the instructions on <a href="{{base_path}}/install-and-setup/setup/distributed-deployment/deploying-wso2-api-m-in-a-distributed-setup">Setting up a Distributed API-M deployment</a>.

### Integration cluster

The integration cluster consist of a minimum of two nodes of the integration runtime (Micro Integrator/Streaming Integrator). See the following links for instructions on how to set up this cluster.

<ul>
    <li>
        <a href="https://mi.docs.wso2.com/en/4.3.0/install-and-setup/setup/deployment/deploying-wso2-mi/">Micro Integrator Cluster with Minimum High Availability</a>
    </li>
    <li>
        <a href="https://ei.docs.wso2.com/en/latest/streaming-integrator/setup/deploying-si-as-minimum-ha-cluster/">Streaming Integrator Cluster with Minimum High Availability</a>
    </li>
</ul>
