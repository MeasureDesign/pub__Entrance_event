![MeasureDesign logo](imgs/md_logo.png)

You can find an article about this configuration **[here](https://www.measuredesign.cz/en/blog/)** on our blog.

# Entrance_event
Reusable Google Tag Manager configuration for detecting website entrances, identifying traffic sources (source, medium, campaign), and sending entrance events to server-side GTM.

The configuration distinguishes website entrances from internal navigation and page reloads. It uses URL parameters and referrer information to classify incoming traffic and sends the resulting anonymous_entrance event using the Stape Data Tag.

## Installation
1. Download the GTM container JSON file from this repository.
2. Open your **Google Tag Manager** web container.
3. Navigate to **Admin → Import Container**.
4. Select the downloaded JSON file.
5. Choose an existing or new workspace.
6. Select **Merge** to preserve your existing container configuration.
7. Review the proposed changes and confirm the import.

**Important:** Before publishing the container, configure the variables described below. The imported configuration contains placeholder values and will not send events to your sGTM server until the endpoint is configured.


## Configuration
### Required variables
| GTM variable | Description |
| ------------ | ----------- |
| config.entrance.internalDomains | Your website's domain(s). Used to distinguish internal navigation from external entrances. |
| cons-sgtmEndpoint | URL of your server-side GTM endpoint, e.g. https://sgtm.yourdomain.com. |

### Domain configuration
Specify domains as comma-separated hostnames, without protocols, paths or ports.
 
Example:

`example.com,example.org`

A configured domain also covers its subdomains.

### Server-side GTM endpoint
Replace https://sgtm.example.com in `cons-sgtmEndpoint` with your actual server endpoint. Check any configured fallback values in the variable's *Format Value* settings as well.

The included Stape Data Tag sends requests to the /data path and requires a compatible Stape Data Client in your server-side GTM container.

### Optional variables
| GTM variable | Description |
| ------------ | ----------- |
| config.entrance.sharedExcludedDomains | Shared exclusion list for domains such as payment gateways and authentication services. |
| config.entrance.extraExcludedDomains | Additional deployment-specific domains that should not be treated as external entrances. |
| lookup.source | Hostname-based classification rules for traffic sources, including search engines and social platforms. |
| lookup.medium | Hostname-based classification rules for traffic media, including organic, social and referral traffic. |

The exclusion lists accept comma-separated hostnames. Review the supplied defaults and extend or modify them as appropriate.

The source and medium lookup variables contain starter classification rules that can be customized for your reporting requirements.


## Testing
Before publishing, use GTM Preview to verify the configuration.

Recommended test scenarios:

| Scenario | Expected result |
| -------- | --------------- |
| Direct navigation to the website | Entrance event fires |
| Navigation from an external website | Entrance event fires |
| Navigation between internal pages | Entrance event does not fire |
| Page reload | Entrance event does not fire |
| Back/forward browser navigation | Entrance event does not fire |
| Navigation from an excluded domain | Entrance event does not fire |
| Entrance with UTM parameters | Event includes the supplied attribution values |

Check the DOM.entrance trigger and the ANON | anonymous_entrance tag in GTM Preview.

The implementation is based on browser document navigation and does not automatically detect client-side route changes in single-page applications (SPAs).


## Additional notes
- This repository contains the web GTM configuration only. The server-side GTM container, Data Client and downstream processing are not included.
- The implementation does not establish or manage user sessions.
- Referrer information may be unavailable due to browser privacy settings, referrer policies or other restrictions.
- Consent requirements and data processing must be configured according to the deployment's applicable requirements.
- The generated entrance_id identifies an event, not a persistent visitor.


## License
This project is licensed under the MIT License.



