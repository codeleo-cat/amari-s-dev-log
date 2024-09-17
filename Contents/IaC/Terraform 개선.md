
- tree
- var > yaml > context + template file

https://developer.hashicorp.com/terraform/language/functions/templatefile


// Add vpc endpoint to route table of private subnet

// 443 허용 필요
resource "aws_security_group_rule" "vpc_endpoint_ssm_messages_https" {

  cidr_blocks       = ["${var.cidr}"]
  type              = "ingress"
  protocol          = "tcp"
  from_port         = 443
  to_port           = 443
  security_group_id = "${aws_security_group.ssmmessages.id}"
}


### 1) Terraform 사용 및 개선 
- [ ] key vault secret 의 경우, 해당 값이 있으면 read / 없으면 생성하는 문법 리서치 후 적용
- [ ] Terraform workspace 관리 방법 논의

### 2) 잔존 이슈 및 고려할 부분
- [ ] self - signed certificate 적용 시 에러 
