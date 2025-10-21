# Full Implementation Notes

(Expanded theoretical foundation, validation matrix, and compliance mapping)

## Defence in Depth rationale

includes least privilege, segmentation, automated enforcement)...

## Validation matrix

* Control: Network Isolation

  * Test: Confirm no public IP on RDS, security group rules only allow app-sg and bastion-sg
  * Expected: RDS not reachable from internet

* Control: IAM Authentication

  * Test: Generate token and connect
  * Expected: Successful connection with token and SSL



