# crossplane-iam-pod-role

![Version: 0.0.6](https://img.shields.io/badge/Version-0.0.6-informational?style=for-the-badge)
![Type: application](https://img.shields.io/badge/Type-application-informational?style=for-the-badge)
![AppVersion: 1.0.0](https://img.shields.io/badge/AppVersion-1.0.0-informational?style=for-the-badge)
[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/crossplane-iam-pod-role)](https://artifacthub.io/packages/search?repo=crossplane-iam-pod-role)
## Description

this chart will create Aws Iam role, policy and service account that every pod can easily assume and use. (using Crossplane)

**crossplane-iam-pod-role** is a kubernetes chart that is an aggregation of *crossplane crd manifests with service account*.

It allows for k8s applications to manage and create their own Aws cloud permissions. It uses [Crossplane](https://github.com/crossplane/crossplane) as its main crd manifests, an [Aws OIDC identity provider](https://aws.amazon.com/blogs/containers/introducing-oidc-identity-provider-authentication-amazon-eks/) is the natively Eks way of assuming Aws Iam roles, we combine both into 1 easy single helm chart for utilize the permissions.

## Prerequisite of this chart

1. Please follow the [Oidc provider](https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html) docs and install it into your cluster
2. Please follow the [Crossplane](https://artifacthub.io/packages/helm/crossplane/crossplane) docs and install it into your cluster.
   once installed, make sure to add Iam full permissions to your ProviderConfig.
## Install using Helm chart

- Configure the below values
    ```yaml

    # -- # Defines the role name prefix.
    # -- # Part of the iam role/policy name that will be created
    role_name_prefix: "crossplane-eks-"

    # -- # Defines the pod name that will assume the permissions.
    # -- # Part of the iam role/policy name that will be created
    pod_name: "my-pod-name"

    # -- # Defines the cluster name that we are using.
    # -- # Part of the iam role/policy name that will be created
    cluster_name: "my-cluster-name"

    # -- # Defines the service account that will be created
    service_account:
      create: true
    # -- # Defines the imagePullSecrets atributes for the service account. (not required) 
      imagePullSecrets:
        - name: my-secret

    # -- # Aws configurations
   
    # -- # Aws account id for the Iam role trust relationship policy
    aws_account_id: "1234567890"
    # -- # Aws Eks openId connect id for the Iam role trust relationship policy
    aws_eks_openId_connect_number: "1111111111222222222GGGGGGGGGPPPPPPPPPP"
    # -- # Aws region for the Iam role trust relationship policy
    aws_region: eu-west-1

    # -- # Defines the Iam policies that will be created and attached to the Iam role
    policies:
        policydocument1:
            {
                "Id": "crossplane-eks-policydocument1",
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Action": [
                            "iam:CreateServiceLinkedRole"
                        ],
                        "Condition": {
                            "StringEquals": {
                                "iam:AWSServiceName": [
                                    "elasticloadbalancing.amazonaws.com"
                                ]
                            }
                        },
                        "Effect": "Allow",
                        "Resource": "*",
                        "Sid": ""
                    }
                ]
            }
        policydocument2:
            {
                "Id": "crossplane-eks-policydocument2",
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Action": [
                            "iam:CreateServiceLinkedRole"
                        ],
                        "Condition": {
                            "StringEquals": {
                                "iam:AWSServiceName": [
                                    "elasticloadbalancing.amazonaws.com"
                                ]
                            }
                        },
                        "Effect": "Allow",
                        "Resource": "*",
                        "Sid": ""
                    }
                ]
            }
    # -- # Defines the name of the crossplane provider-config,
    # -- # which should be predefined with Iam full access
    # -- # for more info, look at aws-provider docs below
    provider_config_name: aws-provider
    # -- # Iam role and policies Aws tags
    tags:
        Component: k8s
        Environment: dev
        ManageBy: crossplane-platform-dev
        Name: crossplane-k8s-aws-load-balancer-controller
        Team: RND
        Type: Platform
    ```

    [aws-provider docs](https://github.com/crossplane/crossplane/blob/master/docs/cloud-providers/aws/aws-provider.md)

- Install latest version of crossplane-iam-pod-role helm chart

    ```
    $ helm repo add crossplane-iam-pod-role https://explorium-ai.github.io/crossplane-iam-pod-role/
    $ helm repo update
    $ helm install crossplane-iam-pod-role/crossplane-iam-pod-role -f values.yaml
    ```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| aws_account_id | string | `"1234567890"` |  |
| aws_eks_openId_connect_number | string | `"1111111111222222222GGGGGGGGGPPPPPPPPPP"` |  |
| aws_region | string | `"eu-west-1"` |  |
| cluster_name | string | `"platform-dev"` |  |
| pod_name | string | `"my-pod-name"` |  |
| policies.policydocument1.Id | string | `"crossplane-eks-policydocument1"` |  |
| policies.policydocument1.Statement[0].Action[0] | string | `"iam:CreateServiceLinkedRole"` |  |
| policies.policydocument1.Statement[0].Condition.StringEquals.iam:AWSServiceName[0] | string | `"elasticloadbalancing.amazonaws.com"` |  |
| policies.policydocument1.Statement[0].Effect | string | `"Allow"` |  |
| policies.policydocument1.Statement[0].Resource | string | `"*"` |  |
| policies.policydocument1.Statement[0].Sid | string | `""` |  |
| policies.policydocument1.Version | string | `"2012-10-17"` |  |
| policies.policydocument2.Id | string | `"crossplane-eks-policydocument2"` |  |
| policies.policydocument2.Statement[0].Action[0] | string | `"iam:CreateServiceLinkedRole"` |  |
| policies.policydocument2.Statement[0].Condition.StringEquals.iam:AWSServiceName[0] | string | `"elasticloadbalancing.amazonaws.com"` |  |
| policies.policydocument2.Statement[0].Effect | string | `"Allow"` |  |
| policies.policydocument2.Statement[0].Resource | string | `"*"` |  |
| policies.policydocument2.Statement[0].Sid | string | `""` |  |
| policies.policydocument2.Version | string | `"2012-10-17"` |  |
| provider_config_name | string | `"aws-provider"` |  |
| role_name_prefix | string | `"crossplane-eks-"` |  |
| service_account.create | bool | `true` |  |
| service_account.imagePullSecrets[0].name | string | `"expregistrykey"` |  |
| tags.Component | string | `"k8s"` |  |
| tags.Environment | string | `"dev"` |  |
| tags.ManageBy | string | `"crossplane-platform-dev"` |  |
| tags.Name | string | `"crossplane-k8s-aws-load-balancer-controller"` |  |
| tags.Team | string | `"RND"` |  |
| tags.Type | string | `"Platform"` |  |

**Homepage:** <https://www.explorium.ai/>

## Source Code

* <https://github.com/explorium-ai>

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| Explorium's Devops Team | <devops@explorium.ai> | <https://github.com/explorium-ai> |