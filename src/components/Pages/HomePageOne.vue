<template>
    <div>
        <Navbar />
        <MainBanner />
        <div id="service-three">
        <ServicesTwo />
        </div>
        <div id="feature">
        <Features />
        </div>
        <HostingServices />
        <div id="why-choose-us">
        <WhyChooseUs />
        </div>

        <Blog />
        <Footer />
    </div>
</template>

<script>
import Navbar from '../Layouts/Navbar';
import MainBanner from '../HomeOne/MainBanner';
import Features from '../Common/Features';
import HostingServices from '../HomeOne/HostingServices';
import WhyChooseUs from '../Common/WhyChooseUs';
import AmazingFeatures from '../HomeOne/AmazingFeatures';
import PricingPlan from '../Common/PricingPlan';
import ReadyToHelpYou from '../Common/ReadyToHelpYou';
import OurLovingClients from '../Common/OurLovingClients';
import Faq from '../Common/Faq'
import BestSupport from '../HomeOne/BestSupport'
import Blog from '../../components/BlogOne/Blog';
import ServicesTwo from '../ServicesThree/ServicesTwo'
import Footer from '../Layouts/Footer'

export default {
    name: 'HomePageOne',
    components: {
        Navbar,
        MainBanner,
        Features,
        HostingServices,
        WhyChooseUs,
        AmazingFeatures,
        PricingPlan,
        ReadyToHelpYou,
        OurLovingClients,
        Faq,
        BestSupport,
        Blog,
        Footer,
        ServicesTwo
    }
};
</script>