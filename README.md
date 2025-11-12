# Lab 04: Implement Virtual Networking

This lab demonstrates the implementation of Azure virtual networking infrastructure, including virtual networks, subnets, security groups, and DNS zones. The lab is structured to accommodate existing resources while planning for organizational growth.

## Lab Overview

- **Estimated Time**: 50 minutes
- **Resource Group**: `az104-rg4`
- **Primary Region**: East US
- **Key Components**: Virtual Networks, Subnets, NSGs, ASGs, DNS Zones

## Architecture

The lab implements two virtual networks:
- **CoreServicesVnet** (10.20.0.0/16): Hosts core services with room for significant growth
- **ManufacturingVnet** (10.30.0.0/16): Supports manufacturing operations with connected IoT devices

Both networks follow best practices by avoiding overlapping IP address ranges to reduce issues and simplify troubleshooting.

---

## Task 1: Create a virtual network with subnets using the portal

In this task, I created the CoreServicesVnet virtual network through the Azure portal. This network is designed to accommodate the largest number of resources and anticipated growth for core services.

### Steps Completed

1. **Azure Portal Overview**
   I started by accessing the Azure portal to begin the virtual network creation process.
   ![Azure Portal Overview](screenshots/Create%20a%20virtual%20network%20with%20subnets%20using%20the%20portal/azure%20portal%20overview.png)

2. **Create Virtual Network | Basics Blade**
   I opened the virtual network creation wizard and landed on the Basics configuration tab, ready to specify the fundamental settings.
   ![Create Virtual Network Basics Blade](screenshots/Create%20a%20virtual%20network%20with%20subnets%20using%20the%20portal/Create%20virtual%20network%20basics%20blade.png)

3. **Create Virtual Network Overview**
   The wizard presented an overview of the configuration steps required to complete the virtual network setup.
   ![Create Virtual Network Overview](screenshots/Create%20a%20virtual%20network%20with%20subnets%20using%20the%20portal/create%20virtual%20network%20overview.png)

4. **Virtual Network IP Address Blade**
   I navigated to the IP Addresses tab to configure the address space and create subnets. This is where I defined the network topology.
   ![Virtual Network IP Address Blade](screenshots/Create%20a%20virtual%20network%20with%20subnets%20using%20the%20portal/virtual%20network%20IP%20address%20blade.png)

5. **CoreServicesVnet Address Space**
   I configured the IPv4 address space as 10.20.0.0/16, providing 65,536 IP addresses to accommodate current resources and future growth.
   ![CoreServicesVnet Address Space](screenshots/Create%20a%20virtual%20network%20with%20subnets%20using%20the%20portal/CoreServicesVnet%20address%20space.png)

6. **Add SharedServicesSubnet**
   I created the SharedServicesSubnet with the starting address 10.20.10.0/24, allocating 256 addresses (251 usable after Azure's reserved addresses).
   ![Add SharedServicesSubnet](screenshots/Create%20a%20virtual%20network%20with%20subnets%20using%20the%20portal/add%20SharedServicesSubnet.png)

7. **SharedServicesSubnet and DatabaseSubnet Created**
   After creating both subnets, I verified that SharedServicesSubnet (10.20.10.0/24) and DatabaseSubnet (10.20.20.0/24) were properly configured.
   ![SharedServicesSubnet and DatabaseSubnet Created](screenshots/Create%20a%20virtual%20network%20with%20subnets%20using%20the%20portal/SharedServicesSubnet%20and%20DatabaseSubnet%20%20created.png)

8. **Add DatabaseSubnet**
   I configured the DatabaseSubnet with starting address 10.20.20.0/24 to isolate database resources within the virtual network.
   ![Add DatabaseSubnet](screenshots/Create%20a%20virtual%20network%20with%20subnets%20using%20the%20portal/add%20DatabaseSubnet.png)

9. **Virtual Network Security Blade**
   I reviewed the security options available, including BastionHost, DDoS Protection, and Firewall settings for the virtual network.
   ![Virtual Network Security Blade](screenshots/Create%20a%20virtual%20network%20with%20subnets%20using%20the%20portal/virtual%20network%20security%20blade.png)

10. **Virtual Network Tags Blade**
    I accessed the Tags blade to add metadata for resource organization and cost management (tags can be added as needed).
    ![Virtual Network Tags Blade](screenshots/Create%20a%20virtual%20network%20with%20subnets%20using%20the%20portal/virtual%20network%20tags%20blade.png)

11. **CoreServicesVnet Validation Passed**
    The Azure portal validated all configurations successfully, confirming that the settings met all requirements before deployment.
    ![CoreServicesVnet Validation Passed](screenshots/Create%20a%20virtual%20network%20with%20subnets%20using%20the%20portal/CoreServicesVnet%20validation%20passed.png)

12. **CoreServicesVnet Deployment Complete**
    The deployment completed successfully. I clicked "Go to resource" to verify the created virtual network.
    ![CoreServicesVnet Deployment Complete](screenshots/Create%20a%20virtual%20network%20with%20subnets%20using%20the%20portal/CoreServicesVnet%20deployment%20complete.png)

13. **CoreServicesVnet Overview**
    I reviewed the virtual network overview, confirming the address space (10.20.0.0/16) and verifying the resource was properly created in the az104-rg4 resource group.
    ![CoreServicesVnet Overview](screenshots/Create%20a%20virtual%20network%20with%20subnets%20using%20the%20portal/CoreServicesVnet%20overview.png)

14. **CoreServiceVnet Subnets Overview**
    I verified both subnets were successfully created: SharedServicesSubnet (10.20.10.0/24) and DatabaseSubnet (10.20.20.0/24), each showing the correct address ranges.
    ![CoreServiceVnet Subnets Overview](screenshots/Create%20a%20virtual%20network%20with%20subnets%20using%20the%20portal/CoreServiceVnet%20subnets%20overview.png)

15. **CoreServicesVnet Export Template**
    From the Automation section, I accessed the export template feature to capture the virtual network configuration as infrastructure-as-code for reuse.
    ![CoreServicesVnet Export Template](screenshots/Create%20a%20virtual%20network%20with%20subnets%20using%20the%20portal/CoreServicesVnet%20export%20template.png)

### Key Takeaways

- Virtual networks provide isolated network environments in Azure
- Proper IP address planning prevents overlapping ranges and simplifies management
- Every subnet reserves 5 IP addresses for Azure services
- Exporting templates enables infrastructure-as-code practices and consistent deployments

---

## Task 2: Create a virtual network and subnets using a template

In this task, I created the ManufacturingVnet virtual network by modifying and deploying the ARM template exported from Task 1. This demonstrates infrastructure-as-code practices and enables consistent, repeatable deployments.

### Steps Completed

1. **CoreServicesVnet with ManufacturingVnet Comparison**
   I opened both the CoreServicesVnet (10.20.0.0/16) and the target ManufacturingVnet (10.30.0.0/16) specifications to understand the required changes for the template modification.
   ![CoreServicesVnet with ManufacturingVnet](screenshots/Create%20a%20virtual%20network%20and%20subnets%20using%20a%20template/CoreServicesVnet%20with%20ManufacturingVnet.png)

2. **Template Modifications - IP Address Changes (10.20.0.0 to 10.30.0.0)**
   I systematically replaced all occurrences of the CoreServicesVnet address space (10.20.0.0) with the ManufacturingVnet address space (10.30.0.0).
   ![10.20.0.0 with 10.30.0.0](screenshots/Create%20a%20virtual%20network%20and%20subnets%20using%20a%20template/10.20.0.0%20with%2010.30.0.0.png)

3. **Subnet 1 Address Change (10.20.10.0 to 10.30.20.0)**
   I updated the first subnet address from SharedServicesSubnet (10.20.10.0/24) to SensorSubnet1 (10.30.20.0/24).
   ![10.20.10.0 to 10.30.20.0](screenshots/Create%20a%20virtual%20network%20and%20subnets%20using%20a%20template/10.20.10.0%20to%2010.30.20.0.png)

4. **Subnet 2 Address Change (10.20.20.0 to 10.30.21.0)**
   I modified the second subnet address from DatabaseSubnet (10.20.20.0/24) to SensorSubnet2 (10.30.21.0/24).
   ![10.20.20.0 to 10.30.21.0](screenshots/Create%20a%20virtual%20network%20and%20subnets%20using%20a%20template/10.20.20.0%20to%2010.30.21.0.png)

5. **Subnet Name Changes (SharedServicesSubnet to SensorSubnet1)**
   I renamed SharedServicesSubnet to SensorSubnet1 throughout the template to reflect the manufacturing environment's IoT sensor infrastructure.
   ![SharedServicesSubnet to SensorSubnet1](screenshots/Create%20a%20virtual%20network%20and%20subnets%20using%20a%20template/SharedServicesSubnet%20to%20SensorSubnet1.png)

6. **Subnet Name Changes (DatabaseSubnet to SensorSubnet2)**
   I renamed DatabaseSubnet to SensorSubnet2 to accommodate additional sensor devices in the manufacturing network.
   ![DatabaseSubnet to SensorSubnet2](screenshots/Create%20a%20virtual%20network%20and%20subnets%20using%20a%20template/DatabaseSubnet%20to%20SensorSubnet2.png)

7. **Edit Parameters File**
   I opened the parameters.json file to update the parameter values specific to the ManufacturingVnet deployment.
   ![Edit parameters](screenshots/Create%20a%20virtual%20network%20and%20subnets%20using%20a%20template/edit%20parameters.png)

8. **Custom Deployment - Basic Blade**
   In the Azure portal, I navigated to "Deploy a custom template" and accessed the basic configuration blade to begin the deployment process.
   ![Custom deployment basic blade](screenshots/Create%20a%20virtual%20network%20and%20subnets%20using%20a%20template/custom%20deployment%20basic%20blade.png)

9. **Custom Deployment Overview**
   The custom deployment interface presented options to build a template in the editor or select from common templates.
   ![Custom deployment overview](screenshots/Create%20a%20virtual%20network%20and%20subnets%20using%20a%20template/custom%20deployment%20overview.png)

10. **Load Template JSON File**
    I selected "Build your own template in the editor" and loaded the modified template.json file containing the ManufacturingVnet configuration.
    ![Template json loaded](screenshots/Create%20a%20virtual%20network%20and%20subnets%20using%20a%20template/template%20json%20loaded.png)

11. **Review and Create**
    After loading both template and parameters, I reviewed the final configuration to ensure all values were correct before initiating deployment.
    ![Review and create](screenshots/Create%20a%20virtual%20network%20and%20subnets%20using%20a%20template/review%20and%20create.png)

12. **Deployment Complete**
    The template deployment completed successfully, creating the ManufacturingVnet with both SensorSubnet1 and SensorSubnet2.
    ![Deployment complete](screenshots/Create%20a%20virtual%20network%20and%20subnets%20using%20a%20template/deployment%20complete.png)

13. **ManufacturingVnet Created**
    I verified the ManufacturingVnet was successfully deployed and appeared in the resource group az104-rg4.
    ![ManufacturingVnet created](screenshots/Create%20a%20virtual%20network%20and%20subnets%20using%20a%20template/ManufacturingVnet%20created.png)

14. **ManufacturingVnet Overview**
    I reviewed the ManufacturingVnet overview to confirm the address space (10.30.0.0/16) and other key properties matched the template specifications.
    ![ManufacturingVnet overview](screenshots/Create%20a%20virtual%20network%20and%20subnets%20using%20a%20template/ManufacturingVnet%20overview.png)

### Key Takeaways

- ARM templates enable consistent and repeatable infrastructure deployments
- Template parameters separate configuration values from infrastructure definitions
- Exporting templates from existing resources accelerates infrastructure-as-code adoption
- Systematic find-and-replace ensures accurate template modifications

---

## Task 3: Create and configure communication between an Application Security Group and a Network Security Group

In this task, I created an Application Security Group (ASG) and Network Security Group (NSG) to implement network segmentation and security controls. The NSG includes an inbound rule allowing traffic from the ASG and an outbound rule denying internet access.

### Steps Completed

1. **Azure Portal Overview**
   I accessed the Azure portal to begin configuring the security groups for the virtual network infrastructure.
   ![Azure portal overview](screenshots/Create%20and%20configure%20communication%20between%20an%20Application%20Security%20Group%20and%20a%20Network%20Security%20Group/azure%20portal%20overview.png)

2. **Create Application Security Group (asg-web)**
   I navigated to Application Security Groups and initiated the creation of asg-web, which will logically group web server resources.
   ![Create application security group asg-web](screenshots/Create%20and%20configure%20communication%20between%20an%20Application%20Security%20Group%20and%20a%20Network%20Security%20Group/create%20application%20security%20group%20asg-web.png)

3. **asg-web Application Security Group Validation Passed**
   The Azure portal validated the ASG configuration, confirming all required fields were properly completed before deployment.
   ![asg-web application security group validation passed](screenshots/Create%20and%20configure%20communication%20between%20an%20Application%20Security%20Group%20and%20a%20Network%20Security%20Group/asg-web%20application%20security%20group%20validation%20passed.png)

4. **Create Network Security Group (myNSGSecure)**
   I created the myNSGSecure network security group to define and enforce network traffic rules for the SharedServicesSubnet.
   ![Create network security group myNSGSecure](screenshots/Create%20and%20configure%20communication%20between%20an%20Application%20Security%20Group%20and%20a%20Network%20Security%20Group/create%20network%20security%20group%20myNSGSecure.png)

5. **Network Security Group Overview**
   I reviewed the NSG blade to understand available configuration options and default settings.
   ![Network security group overview](screenshots/Create%20and%20configure%20communication%20between%20an%20Application%20Security%20Group%20and%20a%20Network%20Security%20Group/network%20security%20group%20overview.png)

6. **Security Group Deployment Complete**
   The NSG deployment completed successfully. I clicked "Go to resource" to configure the security rules.
   ![Security group deployment complete](screenshots/Create%20and%20configure%20communication%20between%20an%20Application%20Security%20Group%20and%20a%20Network%20Security%20Group/security%20group%20deployment%20complete.png)



8. **myNSGSecure Overview**
   I accessed the myNSGSecure overview to review the NSG properties and prepare for subnet association.
   ![myNSGSecure overview](screenshots/Create%20and%20configure%20communication%20between%20an%20Application%20Security%20Group%20and%20a%20Network%20Security%20Group/myNSGSecure%20overview.png)

9. **myNSGSecure Subnet Overview**
   I navigated to the Subnets section to view current associations (initially empty) before linking the NSG to SharedServicesSubnet.
   ![myNSGSecure subnet overview](screenshots/Create%20and%20configure%20communication%20between%20an%20Application%20Security%20Group%20and%20a%20Network%20Security%20Group/myNSGSecure%20subnet%20overview.png)

10. **SharedServicesSubnet Associate Added**
    I successfully associated myNSGSecure with the SharedServicesSubnet (10.20.10.0/24) in CoreServicesVnet, applying the NSG rules to all resources in that subnet.
    ![SharedServicesSubnet associate added](screenshots/Create%20and%20configure%20communication%20between%20an%20Application%20Security%20Group%20and%20a%20Network%20Security%20Group/SharedServicesSubnet%20associate%20added.png)

11. **myNSGSecure Add Associate Subnet**
    I opened the subnet association dialog to select the target virtual network and subnet for the NSG.
    ![myNSGSecure add associate subnet](screenshots/Create%20and%20configure%20communication%20between%20an%20Application%20Security%20Group%20and%20a%20Network%20Security%20Group/myNSGSecure%20add%20associate%20subnet.png)

12. **myNSGSecure Inbound Security Rules**
    I reviewed the default inbound security rules, noting that only virtual network and load balancer traffic is allowed by default.
    ![myNSGSecure inbound security rules](screenshots/Create%20and%20configure%20communication%20between%20an%20Application%20Security%20Group%20and%20a%20Network%20Security%20Group/myNSGSecure%20inbound%20security%20rules.png)

13. **myNSGSecure Inbound Security Rules Overview**
    I examined the complete list of default inbound rules to understand the baseline security posture before adding custom rules.
    ![myNSGSecure inbound security rules overview](screenshots/Create%20and%20configure%20communication%20between%20an%20Application%20Security%20Group%20and%20a%20Network%20Security%20Group/myNSGSecure%20inbound%20security%20rules%20overview.png)

14. **myNSGSecure Add Inbound Security Rule**
    I created a custom inbound rule to allow HTTP (80) and HTTPS (443) traffic from the asg-web Application Security Group with priority 100.
    ![myNSGSecure add inbound security rule](screenshots/Create%20and%20configure%20communication%20between%20an%20Application%20Security%20Group%20and%20a%20Network%20Security%20Group/myNSGSecure%20add%20inbound%20security%20rule.png)

15. **myNSGSecure Validation Passed**
    The Azure portal validated the new inbound security rule configuration, confirming all parameters were correctly specified.
    ![myNSGSecure validation passed](screenshots/Create%20and%20configure%20communication%20between%20an%20Application%20Security%20Group%20and%20a%20Network%20Security%20Group/myNSGSecure%20validation%20passed.png)

16. **myNSGSecure Outbound Security Rules**
    I navigated to the outbound security rules to review default rules allowing internet access (AllowInternetOutbound with priority 65001).
    ![myNSGSecure outbound security rules](screenshots/Create%20and%20configure%20communication%20between%20an%20Application%20Security%20Group%20and%20a%20Network%20Security%20Group/myNSGSecure%20outbound%20security%20rules.png)

17. **myNSGSecure Outbound Security Rules Overview**
    I examined all default outbound rules to understand which traffic is permitted before implementing restrictions.
    ![myNSGSecure outbound security rules overview](screenshots/Create%20and%20configure%20communication%20between%20an%20Application%20Security%20Group%20and%20a%20Network%20Security%20Group/myNSGSecure%20outbound%20security%20rules%20overview.png)

18. **myNSGSecure Add Outbound Security Rule**
    I created a deny rule for internet-bound traffic with priority 4096, overriding the default allow rule (priority 65001) to restrict outbound internet access.
    ![myNSGSecure add outbound security rule](screenshots/Create%20and%20configure%20communication%20between%20an%20Application%20Security%20Group%20and%20a%20Network%20Security%20Group/myNSGSecure%20add%20outbound%20security%20rule.png)

### Key Takeaways

- Application Security Groups enable micro-segmentation by grouping VMs based on application tiers
- Network Security Groups provide stateful firewall capabilities at the subnet or NIC level
- NSG rules are processed by priority (lower numbers first), allowing fine-grained control
- Combining ASGs with NSGs simplifies security rule management for dynamic environments

---

## Task 4: Configure public and private Azure DNS zones

In this task, I configured both public and private DNS zones to enable name resolution for internal and external resources. Public DNS zones resolve names over the internet, while private DNS zones provide name resolution within virtual networks.

### Steps Completed

1. **DNS Zones Overview**
   I accessed the DNS zones blade in the Azure portal to begin creating public and private DNS zones for the organization.
   ![DNS zones overview](screenshots/Configure%20public%20and%20private%20Azure%20DNS%20zones/DNS%20zones%20overview.png)

2. **Create a DNS Zone**
   I initiated the creation of a public DNS zone to host DNS records that resolve to public IP addresses.
   ![Create a DND Zone](screenshots/Configure%20public%20and%20private%20Azure%20DNS%20zones/create%20a%20DND%20Zone.png)

3. **cheptumo.com Validation Pass**
   The Azure portal validated the public DNS zone configuration (using domain cheptumo.com), confirming all required settings were properly specified.
   ![cheptumo com validation pass](screenshots/Configure%20public%20and%20private%20Azure%20DNS%20zones/cheptumo%20com%20validation%20pass.png)

4. **cheptumo.com Deployment Complete**
   The public DNS zone deployment completed successfully. I clicked "Go to resource" to add DNS records.
   ![cheptumo com deployment complete](screenshots/Configure%20public%20and%20private%20Azure%20DNS%20zones/cheptumo%20com%20deployment%20complete.png)

5. **cheptumo.com Overview**
   I reviewed the public DNS zone overview, noting the four Azure DNS name servers assigned to the zone for name resolution.
   ![cheptumo com overview](screenshots/Configure%20public%20and%20private%20Azure%20DNS%20zones/cheptumo%20com%20overview.png)

6. **cheptumo.com Recordsets**
   I examined the existing recordsets, which included default NS (name server) and SOA (start of authority) records.
   ![cheptumo com recordsets](screenshots/Configure%20public%20and%20private%20Azure%20DNS%20zones/cheptumo%20com%20recordsets.png)

7. **Add Record Set www**
   I created an A record named "www" pointing to IP address 10.1.1.4 with a TTL of 1 hour to resolve www.cheptumo.com.
   ![add record set www](screenshots/Configure%20public%20and%20private%20Azure%20DNS%20zones/add%20record%20set%20www%20.png)

8. **WindowsTerminal - nslookup Verification**
   I used nslookup from the command line to verify that www.cheptumo.com correctly resolves to 10.1.1.4 through the Azure DNS name servers.
   ![WindowsTerminal_dCQ80W3qob](screenshots/Configure%20public%20and%20private%20Azure%20DNS%20zones/WindowsTerminal_dCQ80W3qob.png)

9. **Private DNS Zones Overview**
   I navigated to Private DNS zones to create an internal DNS zone for name resolution within the virtual networks.
   ![private DNS Zones overview](screenshots/Configure%20public%20and%20private%20Azure%20DNS%20zones/private%20DNS%20Zones%20overview.png)

10. **Create Private DNS Zone**
    I initiated the creation of a private DNS zone named private.cheptumo.com for internal name resolution.
    ![create private DNS Zone](screenshots/Configure%20public%20and%20private%20Azure%20DNS%20zones/create%20private%20DNS%20Zone.png)

11. **Create Private DNS Zone Validation Pass**
    The Azure portal validated the private DNS zone configuration, ensuring all required parameters were correctly specified.
    ![create private DNS Zone validation pass](screenshots/Configure%20public%20and%20private%20Azure%20DNS%20zones/create%20private%20DNS%20Zone%20validation%20pass.png)

12. **Private cheptumo.com Deployment Complete**
    The private DNS zone deployment completed successfully. I proceeded to link it to virtual networks.
    ![private cheptumo com deployment complete](screenshots/Configure%20public%20and%20private%20Azure%20DNS%20zones/private%20cheptumo%20com%20deployment%20complete.png)

13. **Private cheptumo.com Overview**
    I reviewed the private DNS zone overview, noting that no name servers are listed (private zones don't use public name servers).
    ![private cheptumo com overview](screenshots/Configure%20public%20and%20private%20Azure%20DNS%20zones/private%20cheptumo%20com%20overview.png)

14. **Virtual Network Links Overview**
    I accessed the virtual network links section to connect the private DNS zone to virtual networks for automatic DNS registration.
    ![virtual network links overview](screenshots/Configure%20public%20and%20private%20Azure%20DNS%20zones/virtual%20network%20links%20overview.png)

15. **Add Virtual Network Link**
    I created a virtual network link named "manufacturing-link" to connect the private DNS zone with the ManufacturingVnet.
    ![add virtual network link](screenshots/Configure%20public%20and%20private%20Azure%20DNS%20zones/add%20virtual%20network%20link.png)

16. **Private cheptumo.com Virtual Network Links**
    I verified the manufacturing-link was successfully created, enabling DNS resolution between the private zone and ManufacturingVnet.
    ![private cheptumo com virtual network links](screenshots/Configure%20public%20and%20private%20Azure%20DNS%20zones/private%20cheptumo%20com%20virtual%20network%20links.png)

17. **Private cheptumo.com Recordset ,Add Record Set sensorvm**
    I created an A record named "sensorvm" pointing to 10.1.1.4 to enable internal name resolution for manufacturing virtual machines.
    I reviewed the recordsets in the private DNS zone, which included only the default SOA record initially.
    ![private cheptumo com recordset](screenshots/Configure%20public%20and%20private%20Azure%20DNS%20zones/private%20cheptumo%20com%20recordset.png)



### Key Takeaways

- Public DNS zones resolve domain names over the internet using Azure's name servers
- Private DNS zones provide name resolution exclusively within linked virtual networks
- Virtual network links connect private DNS zones to VNets for automatic VM registration
- DNS record TTL (Time To Live) values control how long records are cached

---

## Lab Summary

This lab demonstrated comprehensive Azure virtual networking implementation across four key areas:

1. **Portal-based Virtual Network Creation**: Created CoreServicesVnet with two subnets using the Azure portal interface
2. **Template-based Deployment**: Leveraged ARM templates to deploy ManufacturingVnet, demonstrating infrastructure-as-code practices
3. **Network Security**: Implemented Application Security Groups and Network Security Groups with custom inbound and outbound rules
4. **DNS Configuration**: Configured both public and private DNS zones for internal and external name resolution

### Skills Demonstrated

- Virtual network design and IP addressing
- Subnet segmentation and planning for growth
- ARM template modification and deployment
- Network security group configuration
- Application security group implementation
- Public and private DNS zone management
- Infrastructure-as-code best practices

### Best Practices Applied

- Avoided overlapping IP address ranges across virtual networks
- Reserved capacity for organizational growth in address space planning
- Implemented least-privilege security with targeted NSG rules
- Separated infrastructure code (template) from configuration values (parameters)
- Used meaningful naming conventions for resources and subnets
- Documented deployments with exported templates for repeatability

---

## Resources

- [Azure Virtual Networks Documentation](https://docs.microsoft.com/azure/virtual-network/)
- [Network Security Groups Overview](https://docs.microsoft.com/azure/virtual-network/network-security-groups-overview)
- [Azure DNS Documentation](https://docs.microsoft.com/azure/dns/)
- [ARM Template Best Practices](https://docs.microsoft.com/azure/azure-resource-manager/templates/best-practices)