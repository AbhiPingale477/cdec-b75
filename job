provider "aws" {
    region = "ap-northeast-1"
}

resource "aws_iam_role" "cluster1" {
  name = "eks-cluster-example987"
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = [
          "sts:AssumeRole",
          "sts:TagSession"
        ]
        Effect = "Allow"
        Principal = {
          Service = "eks.amazonaws.com"
        }
      },
    ]
  })
}

resource "aws_iam_role_policy_attachment" "cluster_AmazonEKSClusterPolicy" {
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKSClusterPolicy"
  role       = aws_iam_role.cluster1.name
}


data "aws_vpc" "default" {
    default = true
}
data "aws_subnets" "default"{
    filter {
        name = "vpc-id"
        values = [data.aws_vpc.default.id]
    }
}

resource "aws_eks_cluster" "cluster1" {
    name = "cluster1"
    role_arn = aws_iam_role.cluster1.arn
    access_config {
        authentication_mode = "API"
    }
    version  = "1.35"

    vpc_config {
        subnet_ids = data.aws_subnets.default.ids
    }
    depends_on = [
    aws_iam_role_policy_attachment.cluster_AmazonEKSClusterPolicy
  ]
}
