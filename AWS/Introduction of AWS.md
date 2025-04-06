![[Pasted image 20240805131411.png]]

AWS launched the first `EC2` service in 2006

AWS Dashboard after login
![[Pasted image 20240805152350.png]]
This layout called as Console

### AWS Regions
![[Pasted image 20240805152559.png]]

==Elastic Computing Cloud==`EC2`

EC2 home:
![[Pasted image 20240805153222.png]]
Here Instance mean `Virtual server`
 
`Dashboard > EC2 > Instances(running) > EC2 Creation`
##### Instance Type:
* t - Series `low perfomance`
* m-series`mid perfomance`
* c-series`High Perfomance`

##### Keypair:
If you want some security or need some third party apps at that time we used the keypari features.

#### After Connected Created EC2 server
We try to connect and install apache server
```jsx
yum install httpd -y
```
here,
`yum` is the source like `apt` in linux.
`install` is the action.
`httpd` is the package.
`-y` means yes.


