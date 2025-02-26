<template>
  <div
    class="config-help-panel"
    :class="props?.active ? 'config-help-panel-open' : ''"
  >
    <div class="config-help-close">
      <FeatherButton
        class="button"
        text
        icon=""
        @click="onClose"
      >
        <FeatherIcon
          class="buttonIcon"
          :icon="chevronRight"
        />
      </FeatherButton>
    </div>
    <div class="config-help-header">
      <div class="config-help-title">
        {{ helpText.title }}
      </div>
      <div class="config-help-body">
        <p>{{ helpText.subTitle }}</p>
        <p>{{ helpText.help }}</p>
      </div>
      <a
        :href="helpText.link"
        target="_blank"
        class="config-help-link"
        >{{ helpText.linkCopy }}</a
      >
    </div>
    <div class="config-help-hr"></div>
    <div class="config-help-footer">
      <!-- TODO: What do we want to do with this info? -->
      <!-- <div class="footer-title">HELP US IMPROVE</div>
      <div class="footer-subtitle">Was this information helpful?</div>
      <div class="footer-button" :class="getFooterClickClass()">
        <div class="footer-yes" @click="footerYes">YES</div>
        <div class="footer-no" @click="footerNo">NO</div>
      </div> -->
    </div>
  </div>
</template>

<script
  lang="ts"
  setup
>
import { PropType } from 'vue'

import { FeatherIcon } from '@featherds/icon'
import { FeatherButton } from '@featherds/button'

import ChevronRight from '@featherds/icon/navigation/ChevronRight'
import { RequisitionPluginSubTypes, RequisitionTypes } from './copy/requisitionTypes'
import { LocalConfiguration } from './configuration.types'

/**
 * Props
 */
const props = defineProps({
  active: Boolean,
  onClose: { type: Function as PropType<(payload: MouseEvent) => void> },
  item: { type: Object as PropType<LocalConfiguration>, required: true }
})

/**
 * Local State
 */
const chevronRight = computed(() => ChevronRight)
const footerVals = reactive({ yes: false, no: false })

/**
 * Get the copy for the help window based on the selected
 * type and subtype (if Requisition is selected)
 * This should probably be moved to an API call in the future
 */
const helpText = computed(() => {
  const typeName = props.item.type.name
  const subType = props.item.subType.name

  let helpVals = {
    title: 'External Requisition',
    subTitle: 'An external requisition enables periodic inventory synchronization from external sources. The external requisition contains a URL, schedule, and setting to specify the nodes to rescan. The "type" defines the external source, and uses type-specific parameters (host, path, foreign source) to construct the URL that tells OpenNMS where to find this information.',
    help: 'See the online documentation for detailed information on supported options:',
    linkCopy: 'READ FULL ARTICLE',
    link: 'https://docs.opennms.com/horizon/latest/reference/provisioning/handlers/introduction.html'
  }

  if (typeName === RequisitionTypes.File) {
    helpVals = {
      title: 'File',
      subTitle: 'The file handler imports a properly formatted requisition from an XML file stored locally on the server. Requires the /path parameter to construct the URL OpenNMS uses to locate this external source.',
      help: 'See the online documentation for detailed information on supported options:',
      linkCopy: 'READ FULL ARTICLE',
      link: 'https://docs.opennms.com/horizon/latest/reference/provisioning/handlers/file.html'
    }
  } else if (typeName === RequisitionTypes.RequisitionPlugin) {
    if (subType === '') {
      helpVals = {
        title: RequisitionTypes.RequisitionPlugin,
        subTitle: 'OpenNMS provides plugins that can provision nodes from additional platforms, including software-defined networking, hypervisor, and configuration management database (CMDB) platforms.',
        help: 'See the online documentation for detailed information on supported options:',
        linkCopy: 'READ FULL ARTICLE',
        link: 'https://docs.opennms.com/horizon/latest/reference/provisioning/handlers/introduction.html'
      }
    } else if (subType === RequisitionPluginSubTypes.OpenDaylight) {
      helpVals = {
        title: 'OpenDaylight',
        subTitle: 'The OpenDaylight handler imports a requisition generated by the OpenNMS OpenDaylight plugin. The plugin enables OpenNMS to monitor devices and networks that OpenDaylight controllers manage and import inventory from the operational topology.',
        help: 'See the online documentation for detailed information on supported options:',
        linkCopy: 'READ FULL ARTICLE',
        link: 'https://docs.opennms.com/horizon/latest/reference/provisioning/handlers/introduction.html'
      }
    } else if (subType === RequisitionPluginSubTypes.ACI) {
      helpVals = {
        title: 'ACI',
        subTitle: 'The ACI handler imports a requisition generated by the OpenMMS ACI plugin. The OpenNMS ACI plugin enables OpenNMS to monitor devices and networks that Cisco APIC controllers manage and import inventory from the operational topology.',
        help: 'See the online documentation for detailed information on supported options:',
        linkCopy: 'READ FULL ARTICLE',
        link: 'https://docs.opennms.com/horizon/latest/reference/provisioning/handlers/introduction.html'
      }
    } else if (subType === RequisitionPluginSubTypes.Zabbix) {
      helpVals = {
        title: 'Zabbix',
        subTitle: 'The Zabbix handler imports a requisition generated by the OpenNMS Zabbix plugin. The Zabbix plugin lets OpenNMS integrate with the Zabbix agent and use Zabbix templates to collect requisition data.',
        help: 'See the online documentation for detailed information on supported options:',
        linkCopy: 'READ FULL ARTICLE',
        link: 'https://docs.opennms.com/horizon/latest/reference/provisioning/handlers/introduction.html'
      }
    } else if (subType === RequisitionPluginSubTypes.AzureIot) {
      helpVals = {
        title: 'Azure IoT',
        subTitle: 'The Azure IoT handler imports a requisition generated by the OpenNMS Azure IoT plugin. The Azure IoT plugin can build a requisition from all devices in an IoT hub.',
        help: 'See the online documentation for detailed information on supported options:',
        linkCopy: 'READ FULL ARTICLE',
        link: 'https://docs.opennms.com/horizon/latest/reference/provisioning/handlers/introduction.html'
      }
    } else if (subType === RequisitionPluginSubTypes.PRIS) {
      helpVals = {
        title: 'PRIS',
        subTitle: 'The PRIS (Provisioning Integration Server) handler imports a requisition generated by PRIS. PRIS is a tool that helps you get information from your external inventory into an OpenNMS requisition model. The output from PRIS is provided as XML over HTTP.',
        help: 'See the online documentation for detailed information on supported options:',
        linkCopy: 'READ FULL ARTICLE',
        link: 'https://docs.opennms.com/pris/1.3.2/index.html'
      }
    }
  } else if (typeName === RequisitionTypes.VMWare) {
    helpVals = {
      title: RequisitionTypes.VMWare,
      subTitle: 'The VMware adapter pulls hosts and/or virtual machines from a vCenter server into OpenNMS. With this adapter, you can automatically add, update, or remove nodes from your inventory based on the status of the VMware entity. Uses the host, optional username, and optional password parameters to construct the URL OpenNMS uses to locate this external source. If username and password are left blank, the system will rely on credentials configured in vmware-config.xml.',
      help: 'See the online documentation for detailed information on supported options:',
      linkCopy: 'READ FULL ARTICLE',
      link: 'https://docs.opennms.com/horizon/latest/reference/provisioning/handlers/vmware.html'
    }
  } else if (typeName === RequisitionTypes.HTTP || typeName === RequisitionTypes.HTTPS) {
    helpVals = {
      title: 'HTTP(S)',
      subTitle: 'The HTTP(S) handler imports a properly formatted external requisition from an XML file stored on or generated by a web server. Requires the host and /path parameters to construct the URL OpenNMS uses to locate this external source.',
      help: 'See the online documentation for detailed information on supported options:',
      linkCopy: 'READ FULL ARTICLE',
      link: 'https://docs.opennms.com/horizon/latest/reference/provisioning/handlers/http.html'
    }
  } else if (typeName === RequisitionTypes.DNS) {
    helpVals = {
      title: 'DNS',
      subTitle: 'The DNS handler requests a zone transfer (AXFR) from a DNS server to retrieve the A and AAAA records and build an import requisition. Requires the host, zone, and foreign source parameters to construct the URL OpenNMS uses to locate this external source.',
      help: 'See the online documentation for detailed information on supported options:',
      linkCopy: 'READ FULL ARTICLE',
      link: 'https://docs.opennms.com/horizon/latest/reference/provisioning/handlers/dns.html'
    }
  }
  return helpVals
})

/**
 * Gets the current class structure for the
 * two zone click box.
 */
const getFooterClickClass = () => {
  let vals = ''
  if (footerVals.yes) {
    vals = 'footer-wrap-yes'
  }
  if (footerVals.no) {
    vals = 'footer-wrap-no'
  }
  return vals
}

/**
 * Stub for when the user clicks YES on two zone click box.
 * Functionality for this to be determined. Included in design from UX.
 *
 */
const footerYes = () => {
  footerVals.yes = true
  footerVals.no = false
  console.log('The User Has Selected Yes!', props.item)
}

/**
 * Stub for when the user clicks NO on the two zone click box.
 * Functionality for this to be determined. Included in design from UX.
 */
const footerNo = () => {
  console.log('The User Has Selected No!', props.item)
  footerVals.no = true
  footerVals.yes = false
}
</script>

<style lang="scss">
@import "@featherds/styles/themes/variables";

.config-help-header {
  a.config-help-link:visited {
    color: var($secondary-variant);
  }
}
</style>
<style
  lang="scss"
  scoped
>
@import "@featherds/styles/mixins/typography";
@import "@featherds/styles/themes/variables";

.config-help-close {
  display: flex;
  justify-content: flex-end;
  padding-top: 12px;
  height: 50px;
  .button {
    font-size: 42px;
    color: var($secondary-variant);
    display: flex;
    align-items: center;
    margin: 0;
  }
}
.config-help-panel {
  position: fixed;
  background-color: var($background);
  z-index: 5;
  top: 60px;
  right: 0;
  width: 20vw;
  height: calc(100vh - 60px);
  transform: translateX(20vw);
  transition: transform ease-in-out 0.3s;
}
.config-help-panel-open {
  transform: translateX(0vw);
}
.config-help-header {
  padding: 20px 40px;
  a.config-help-link {
    font-weight: 700;
    color: var($secondary-variant);
    margin-top: 40px;
    margin-bottom: 0px;
  }
}
.config-help-title {
  @include headline2();
  color: var($primary);
  margin-top: 32px;
}
.config-help-body {
  @include body-small();
  margin-top: 12px;
  p {
    margin-bottom: 24px;
  }
}
.config-help-hr {
  border-bottom: 1px solid var($primary);
  margin: 0 40px;
  margin-bottom: 80px;
}
.config-help-footer {
  margin: 0 40px;
  .footer-title {
    font-weight: 700;
    color: var($primary);
  }
  .footer-subtitle {
    @include headline4();
    font-weight: 700;
  }
  .footer-button {
    display: flex;
    border: 1px solid var($secondary-variant);
    max-width: 200px;
    text-align: center;
    height: 50px;
    align-items: center;
    margin-top: 34px;
    div {
      width: 100px;
      height: 50px;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
    }
    .footer-yes {
      color: var($primary);
      transition: all ease-in-out 0.3s;
    }
    .footer-no {
      color: var($primary);
      width: 100px;
      transition: all ease-in-out 0.3s;
    }
    &.footer-wrap-yes {
      .footer-yes {
        background-color: var($secondary-variant);
        color: var($surface);
      }
    }
    &.footer-wrap-no {
      .footer-no {
        background-color: var($secondary-variant);
        color: var($surface);
      }
    }
  }
}
</style>

