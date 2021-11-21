
******************
EJBCA Introduction
******************


Introduction
============

EJBCA is one of the longest running CA software projects, providing time-proven robustness and reliability. EJBCA is platform independent, and can easily be scaled out to match the needs of your PKI requirements, whether you're setting up a national eID, securing your industrial IOT platform or managing your own internal PKI. 

EJBCA covers all your needs - from certificate management, registration and enrollment to certificate validation.

Certificate Lifecycle Management
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
EJBCA provides full capabilities for managing your certificate lifecycles, from powerful profiles that give you fine-grained and easily configured control over the identities and properties of your cryptographic certificates, automated validation of submitted keys and certification requests and multiple enrollment vectors through our own Registration Authority UI and all common enrollment protocols, to advanced administrative workflows to ensure that your organization retains control and oversight of your certificates.

EJBCA provides easy to use tools to allow administrators to easily revoke and renew certificates, ensuring that lost keys are immediately contained and that your organization suffers no downtime. 

Integration and DevOps
~~~~~~~~~~~~~~~~~~~~~~
EJBCA is built from the ground up to be easy and painless to deploy and maintain. A frequent release cycle ensures that bugs are quickly fixed and mitigated, and through clustering we allow upgrades to take place over an entire PKI with zero downtime. We have provided migration guides from several legacy PKIs, and integration guides to multiple third-party applications and guides for most Hardware Security Module vendors. 

Dynamic and Scalable
~~~~~~~~~~~~~~~~~~~~~
EJBCA is your one-stop shop, from setting up your own self-contained PKI to setting up a complex infrastructure with 100% uptime requirements and extreme performance demands. EJBCA instances can easily be couple securely over TLS in order to secure your CA infrastructure as much as possible while providing accessibility to registration and validation nodes. By clustering nodes, high levels of reliability and performance can be achieved, achieving high degrees of availability regardless of external circumstances. 



The following sections cover EJBCA concepts and architectures, and provides an overview of EJBCA's capabilities and support:

EJBCA Concepts
~~~~~~~~~~~~~~
EJBCA implements Public Key Infrastructure (PKI) according to standards such as X.509 and IETF-PKIX, and thus follows the general PKI concepts closely. The administration of the PKI includes some EJBCA specific concepts in order to implement unique flexibility. For definitions for general and EJBCA specific concepts and key terms, see EJBCA Concepts.

EJBCA Architecture
~~~~~~~~~~~~~~~~~~
There are multiple ways that you can implement and architect a PKI solution, ranging from simple and low cost, to very complex and costly. EJBCA allows implementing virtually any type of PKI architecture, for information on a selection of common PKI architectures deployed, see EJBCA Architecture.

Interoperability and CertificationsLink to Interoperability and Certifications
For an overview of EJBCA's capabilities and support, with relevant links to documentation and external standards, see Interoperability and Certifications.


EJBCA concepts
==============

The following lists definitions for general and EJBCA specific concepts and key terms. EJBCA implements the Certification Authority (CA) part of a Public Key Infrastructure (PKI) according to standards such as X.509 and IETF-PKIX. As such it follows the general PKI concepts closely. The administration of the PKI has some EJBCA specific concepts in order to implement unique flexibility.

PKI Architectures
~~~~~~~~~~~~~~~~~

First of all, we need to establish some general terms in order to continue.

.. image:: ../images/pki-architecture.png
    :scale: 70 %
    :align: center

* Root CA

A RootCA has a self-signed certificate and is also called Trusted Root. Verification of other certificates in the PKI ends with the RootCAs self-signed certificate. Since the RootCAs certificate is self-signed it must somehow be configured as a trusted root for all clients in the PKI.

* Sub CA

A subordinate CA, or SubCA for short, is a CA whose certificate is signed by another CA, which can be another SubCA or a RootCA. Since the SubCAs certificate is signed by another CA, it does not have to be configured as a trusted root. It is part of a certificate chain that ends in the RootCA.

* Registration Authority (RA)

A Registration Authority (RA) is an administrative function that registers entities in the PKI. The RA is trusted to identify and authenticate entities according to the CAs policy. There can be one or more RAs connected to each CA in the PKI.

* Validation Authority (VA)

A Validation Authority (VA) is responsible for providing information on whether a certificate is currently valid or not. The VA does not issue or revoke certificates, but it validates certificates by providing a list of revoked certificates for a CA, known as a Certificate Revocation List (CRL). Another method that the VA can support is the Online Certificate Status Protocol (OCSP). It is a real-time lookup of a certificate status, compared to the CRL which is generated on a set schedule. The VA can respond to OCSP requests and reply if a certificate is good, revoked, or unknown. There can be one or more VAs connected to each CA in the PKI.

Certificate Related Concepts
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* Certification Authority (CA)

A Certification Authority (CA) issues certificates to and vouches for the authenticity of entities. 

The level of trust you can assign to a CA is individual, per CA, and depends on the CAs Policy (CP) and CA Practices Statement (CPS). 

For more information, see Certificate Authority Overview.

* End Entity

An End Entity is a user of the PKI, like a device, person, or server. It is called the End Entity because in a hierarchy of certificates in the PKI, it is always the end point, since it is not authorized to issue any certificates of its own.

The End Entity individual or device requests a certificate from the CA or RA. One End Entity can hold many certificates, but all of these certificates will have the same identifying values (Subject DN, Subject Alternative Name, etc). 

.. image:: ../images/end-entity.png
    :scale: 70 %
    :align: center

Keep in mind that an End Entity should not be confused with a physical person. From a CA's point of view, an End Entity may be a physical user, but in reality, it is any entity that holds a certificate. This could also be an OCSP Signer or a Sub CA.

.. image:: ../images/subca.png
    :scale: 70 %
    :align: center

Each End Entity is enrolled against one and only one CA, which will be known as the issuer of that End Entity's certificates. 

For more information, see End Entities Overview.

* End Entity Profile

End Entity Profiles define templates for End Entities. An End Entity Profile isn't inherently necessary as EJBCA provides a default profile (named Empty) which provides no constraints, but for almost all PKIs it's useful and often necessary to put some constraints on what values that users may use to enroll for an End Entity. The values defined in End Entity Profiles are those that pertain to the End Entity directly, and by extension into the identifying fields in the certificate except for data regarding keys and signatures, which will be defined in the Certificate Profiles. The typical values defined, besides available Certificate Profiles and available CAs, will be identifying values such as Subject DN (country, organization, common name, etc) and Subject Alternative Names (SAN) such as dnsName.

.. image:: ../images/certificate-profile.png
    :scale: 70 %
    :align: center

Values defined in End Entity Profiles can be pre-set (not modifiable during enrollment), set with a default value but still modifiable, unset but critical (meaning that they have to be filled in during enrollment) or completely optional. End Entity Profiles can be made as specific or as general as you wish, so it may cover everything from a specific End Entity to an entire set. Each End Entity uses exactly one profile, but the same profile may be shared amongst many End Entities.

.. image:: ../images/end-entity-profile-example.png
    :scale: 70 %
    :align: center

For more information, see End Entity Profiles Overview.

Certificate Profile
~~~~~~~~~~~~~~~~~~~

A Certificate Profile is used to configure certain content and constraints of certificates, such as certificate extensions, available algorithms, key sizes, etc. Basically, it describes what an issued certificate is going to be constrained to. 

.. image:: ../images/certificate-extensions.png
    :scale: 70 %
    :align: center

The certificate extensions allow you to define if a specific extension is present and whether it is critical or not. Some extensions are populated with a value, where it is the same value for all certificates such as CRLDistributionPoint. For other extensions only the presence is determined, where the value is user- or cert-specific such as SubjectAlternativeName. Here is also determined if these certificates will be published and with which publisher.

Certificate Profiles are used in multiple places. They're selected in the CA configuration in order to define the nature of certificates of the CA's own keys:

.. image:: ../images/ca-certificate-data.png
    :scale: 70 %
    :align: center

Likewise, several can be picked when configuring an End Entity Profile:

.. image:: ../images/main-certificate-data.png
    :scale: 70 %
    :align: center

The Certificate Profile defined for the CA will not affect the certificates issued to End Entities by that CA - these certificates will instead be defined by the Certificate Profiles chosen in the End Entity Profiles: 

.. image:: ../images/end-entity-profile-example2.png
    :scale: 70 %
    :align: center

Certificate Profiles are meant to be generically defined and shared amongst different End Entity Profiles, meaning that End Entities different enough to warrant separate End Entity Profiles can share the same Certificate Profile. In addition End Entity Profiles can also define several Certificate Profiles, allowing a single End Entity a choice between different types of certificates to have issued to it, alternatively to have several different types of certificates issued. Each certificate is defined by one and only one certificate profile.

For more information, see Certificate Profiles Overview.


