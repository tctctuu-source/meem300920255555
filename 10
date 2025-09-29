import React from 'react';
import { Link } from 'react-router-dom';
import { Facebook, Twitter, Instagram, Mail, Phone, MapPin } from 'lucide-react';

const Footer: React.FC = () => {
  return (
    <footer className="bg-brand-dark-blue text-ui-text-light">
      <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
        <div className="grid grid-cols-1 md:grid-cols-4 gap-8">
          {/* Logo and Description */}
          <div className="col-span-1 md:col-span-2">
            <Link to="/" className="group flex items-center space-x-3 mb-4">
                <div className="w-10 h-10 bg-brand-light-blue rounded-lg flex items-center justify-center transform group-hover:rotate-[-15deg] transition-transform duration-300">
                  <span className="text-brand-dark-blue font-bold text-2xl font-serif transform group-hover:rotate-[15deg] transition-transform duration-300">
                    M
                  </span>
                </div>
                <span className="text-ui-text-light font-bold text-xl font-serif tracking-tight">
                  SSF Muhimmath
                </span>
            </Link>
            <p className="text-ui-text-light/70 mb-4 max-w-md">
              Fostering a generation of compassionate leaders through education, da'wa, and community service.
            </p>
            <div className="flex space-x-4">
              <a href="#" className="text-ui-text-light/70 hover:text-brand-light-blue transition-colors">
                <Facebook className="w-5 h-5" />
              </a>
              <a href="#" className="text-ui-text-light/70 hover:text-brand-light-blue transition-colors">
                <Twitter className="w-5 h-5" />
              </a>
              <a href="#" className="text-ui-text-light/70 hover:text-brand-light-blue transition-colors">
                <Instagram className="w-5 h-5" />
              </a>
            </div>
          </div>

          {/* Quick Links */}
          <div>
            <h3 className="text-lg font-semibold mb-4">Quick Links</h3>
            <ul className="space-y-2">
              <li><Link to="/about" className="text-ui-text-light/70 hover:text-brand-light-blue transition-colors">About Us</Link></li>
              <li><Link to="/gallery" className="text-ui-text-light/70 hover:text-brand-light-blue transition-colors">Gallery</Link></li>
              <li><Link to="/results" className="text-ui-text-light/70 hover:text-brand-light-blue transition-colors">Results</Link></li>
              <li><Link to="/news" className="text-ui-text-light/70 hover:text-brand-light-blue transition-colors">News</Link></li>
              <li><Link to="/contact" className="text-ui-text-light/70 hover:text-brand-light-blue transition-colors">Contact</Link></li>
            </ul>
          </div>

          {/* Contact Info */}
          <div>
            <h3 className="text-lg font-semibold mb-4">Contact Info</h3>
            <div className="space-y-3">
              <div className="flex items-center space-x-2">
                <MapPin className="w-4 h-4 text-brand-light-blue" />
                <span className="text-ui-text-light/70 text-sm">Muhimmathul Muslimeen Education Center Kasaragod</span>
              </div>
              <div className="flex items-center space-x-2">
                <Phone className="w-4 h-4 text-brand-light-blue" />
                <span className="text-ui-text-light/70 text-sm">+91 9400060851</span>
              </div>
              <div className="flex items-center space-x-2">
                <Mail className="w-4 h-4 text-brand-light-blue" />
                <span className="text-ui-text-light/70 text-sm">eyemedia313@gmail.com</span>
              </div>
            </div>
          </div>
        </div>

        <div className="border-t border-brand-mid-blue/50 mt-8 pt-8 text-center text-ui-text-light/70">
          <p>
            Developed By <a href="https://ibrahimkhaleel.vercel.app/" target="_blank" rel="noopener noreferrer" className="text-ui-text-light hover:text-brand-light-blue transition-colors">Ibrahim Khaleel Kattathar</a>.
            <span className="mx-2">|</span>
            <Link to="/admin/login" className="text-ui-text-light hover:text-brand-light-blue transition-colors">Admin</Link>
          </p>
        </div>
      </div>
    </footer>
  );
};

export default Footer;
